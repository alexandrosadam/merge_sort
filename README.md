# merge_sort
Merge_sort implementation in java


 In the main function we read the file,we convert the strings of the file into integers 
  and then pass them through array in order to send in to mergesort class.
 */
package algorithms;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;

public class Algorithms {

    private static int[] convertArraylist(ArrayList<Integer> list) {
        int[] arr = new int[list.size()];
        //fill in the array with numbers from list
        for (int i = 0; i < list.size(); i++) {
            arr[i] = list.get(i);
        }
        return arr;
    }
    public static void main(String[] args) throws IOException {
        if (args.length < 1) {
            System.out.println("Please set input file.");
            System.exit(1);
        }
        String filename = "";
        //try-catch exception which finds the path of the file to be read
        try {
            Path path = Paths.get(args[0]);
            filename = path.normalize().toAbsolutePath().toString();
        } catch (Exception e) {
            System.out.println("Please set input file.");
            System.exit(1);
        }
        int[] arr = null;
        ArrayList<Integer> myArraylist = new ArrayList<>();
        mergesort ms = new mergesort();
        BufferedReader br = null;
        String line;

        try {
            br = new BufferedReader(new FileReader(filename));
        } catch (FileNotFoundException fnfex) {
            System.out.println(fnfex.getMessage() + "The file was not found");
            System.exit(0);
        }
        //This is where we read lines
        try {

            while ((line = br.readLine()) != null) {
                String[] parts = line.split(" ");
                //converting the strings into integers
                for (int i = 0; i < parts.length; i++) {
                   //this try-catch is for the purpose that if the file has more gaps among numbers do not make 
                   //any problem when we are reading the file                  
                    try {
                        myArraylist.add(Integer.parseInt(parts[i]));
                    } catch (Exception exc) {
                        continue;
                    }
                }
            }

            ms.sort(convertArraylist(myArraylist));
       //catch exception during reading the file finds something wrong nad then prints the message
        } catch (IOException ioex) {
            System.out.println(ioex.getMessage() + "Error reading file ");
        } finally {
            System.exit(0);
        }
    }
}
/*
 Time Complexity: Sorting arrays on different machines.
Merge Sort is a recursive algorithm and time complexity can be expressed as following recurrence relation.
T(n) = 2T(n/2) + Θ(n)
The above recurrence can be solved either using Recurrence Tree method or Master method. 
It falls in case II of Master Method and solution of the recurrence is \Theta(nLogn).
Time complexity of Merge Sort is \Theta(nLogn) in all 3 cases (worst, average and best) as merge sort always divides the array
in two halves and take linear time to merge two halves.
 */

/*
I took the mergesort code from this address http://www.vogella.com/tutorials/JavaAlgorithmsMergesort/article.html
from vogella website.
*/
package algorithms;

public class mergesort {

    private int[] arr;
    private int[] temp;
    private int cost;
    public int number;
    
    public mergesort() {
        cost = 0;
    }
    public void print() {
        System.out.println("Total cost : " + cost);
    }
    public void sort(int[] values) {
        this.arr = values;
        number = values.length;
        this.temp = new int[number];
        recursion(0, number - 1);
        print();
    }
    private void recursion(int start, int finish) {
        // check if start is smaller then finish, if not then the array is sorted
        if (start < finish) {
            // Get the index of the element which is in the middle
            int middle = start + (finish - start) / 2;
            // Sort the left side of the array
            recursion(start, middle);
            // Sort the right side of the array
            recursion(middle + 1, finish);
            // Combine them both
            merge_arrays(start, middle, finish);
        }
    }
    /*
    This function  merges the divided arrays into to the main array.
    We compare the elements in while loop , when we find an element from temp[0,...,middle] that is bigger
    than an element from temp[middle+1,...,finish] we switch the positions of the elements and copy them into
    the main array. This method stops until i and j reach the limits values which means they are empty,they don't
    have more elements to compare.The remaining elements from the left side which they are sorted are coppied into the
    main array and then the merging is done.The numbers of the array are sorted.When we switch elements we count the cost,
    cost is three when the numbers which are compared are different more than two and and the cost is two when they are 
    different for one.
    */
    private void merge_arrays(int start, int middle, int finish) {

        // Copy both parts into the helper array
        for (int i = start; i <= finish; i++) {
            temp[i] = arr[i];
        }
        int i = start;
        int j = middle + 1;
        int k = start;
        // Copy the smallest values from either the left or the right side back
        // to the original array
        while (i <= middle && j <= finish) {
            if (temp[i] <= temp[j]) {
                arr[k] = temp[i];
                i++;
            } else {
                if (temp[i] == temp[j] + 1) {
                    cost += 2;
                } else {
                    cost += 3;
                }
                arr[k] = temp[j];
                j++;
            }
            k++;
        }
        // Copy the rest of the left side of the array into the target array
        while (i <= middle) {
            arr[k] = temp[i];
            k++;
            i++;
        }
    }
    public int getCost() {
        return cost;
    }
}
