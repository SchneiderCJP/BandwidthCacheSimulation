
// Imports Array List and Semaphores for use in the code.
import java.util.ArrayList;
import java.util.Scanner;
import java.util.concurrent.Semaphore;


public class Main {
	
	// Creates a semaphore for access to the bandwidth cache, so that only 1/10 thread can access it at a time.
	// It is initialized at the beginning allowing which ever thread calls acquire first to begin their process immediately, while others have to then wait if unavailable.
	static Semaphore BCAccess = new Semaphore(1,true);
	
	//Creates a list to hold all the bandwidth data with indexes going up to the value of 100. 
	// Indexes are used as the destinations and the contents being the pings from 1-10.
	static int [] bcache = new int [101];

// The decision thread class is defined.
static class Decisionthread extends Thread{
	
	// Takes in a name for the thread and assigns it to a variable.
	// The name is used to differentiate the different threads when describing the activity of the threads.
	String Name = "";
	
	// Takes in the user input count for the amount of queries the user would like each thread to make and assigns it to a variable.
	// The variable is used to keep track of a loop the thread's decision process.
	int count = 0;
	
	Decisionthread(String Name, int count){
		this.Name = Name;
		this.count = count;
	}
	
	// An array list to hold all the strings of data that will be displayed on the console at the end of each query. 
	ArrayList<String> finished = new ArrayList<String>();
	
	
	// To run the thread.
	@Override
	public void run() {
		
		
		
		// A for loop using the count variable, to make the thread run through the decision process a set amount of time.
		for (int c = count; c != 0; c--) {
			
			// Generates a random number between 1-100 for the destination address that the thread would like to look into.
			int dVal = (int)(Math.random()*(100-1+1)+1);
			
			// Displays information about the thread and the destination address it will look into.
			finished.add("");
			finished.add("");
			finished.add("");
			finished.add("*************************************************");
			finished.add("Decision Thread: " + Name);
			
			finished.add("*************************************************");
			finished.add("New decision query in the bandwidth cache:");
			finished.add("Decision thread " + Name + " will query the destination " + dVal + ".");
			
			// The thread tries to acquire access to the bandwidth cache, if it is available, the thread will continue, if it is not it will have to wait till it's turn.
			try {
				BCAccess.acquire();
				
			} catch (InterruptedException e) {
				
			}
			
			// Once the thread has acquired access to the bandwidth cache it can check the destination address.
			// If the bandwidth address (the index) in the bandwidth cache (bcache) is = 0, then that means there is no destination for the address in the bandwidth.
			if (bcache[dVal] == 0) {
				
				// It takes a number from 1-10, representing the ping and assigns it to a variable.
				int ping = (int)(Math.random()*(10-1+1)+1);
				
				// It places the variable at the destination address in the bandwidth cache.
				bcache[dVal] = ping;
				
				// Information about the process is added to the finished array for output once the overall query is finished. 
				finished.add("Destination bandwidth query is performed.");
				finished.add("It took a time of "+ ping + " for a ping. This value was added to the bandwidth cache at " + dVal + ".");
			} 
			
			// If it is not = 0, that means there is a destination for that address and the thread reads it.
			
			// The last thing the thread does is read what is at the destination and continues on with it's process.
			// This information is added to the finished arraylist for output at the end of the query.
			finished.add("It reads " + bcache[dVal] + " at the destination " + dVal + " and returns back to its decision process.");
			
			// For loop goes through the finished arraylist to display all the information of the events that took place during the overall query.
			for (int i = 0; i != finished.size(); i++) {
				System.out.println(finished.get(i));
			}
			
			// The finished arraylist is cleared so that the next possible query of the thread can utilize it.
			finished.clear();
			
			// The access to the bandwidth cache is released now that the thread is done using it. 
			BCAccess.release();

			
			
		}
		
		
	}
}



	public static void main(String[] args) {
		
		// Variable for the count of how many queries the user would like for all of the threads to do.
		int count = 0;
		
		// Scanner is initiated to analyze the user's input.
		Scanner scan = new Scanner(System.in);
		
		// Asks the user how many queries they would like and takes in the number.
		System.out.println("How many queries would you like each thread to make? (Input 0 to stop): ");
		count = scan.nextInt();	
		
		// Each of the 10 threads are initialized and started with a name and the count sent in.
		Decisionthread DT1 = new Decisionthread("One", count);
		Decisionthread DT2 = new Decisionthread("Two", count);
		Decisionthread DT3 = new Decisionthread("Three", count);
		Decisionthread DT4 = new Decisionthread("Four", count);
		Decisionthread DT5 = new Decisionthread("Five", count);
		Decisionthread DT6 = new Decisionthread("Six", count);
		Decisionthread DT7 = new Decisionthread("Seven", count);
		Decisionthread DT8 = new Decisionthread("Eight", count);
		Decisionthread DT9 = new Decisionthread("Nine", count);
		Decisionthread DT10 = new Decisionthread("Ten", count);
		DT1.start();
		DT2.start();
		DT3.start();
		DT4.start();
		DT5.start();
		DT6.start();
		DT7.start();
		DT8.start();
		DT9.start();
		DT10.start();
		
		// Scanner is closed. 
		scan.close();
		
		
		
		
	
		
		
		
	}

}
