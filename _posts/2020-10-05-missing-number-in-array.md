---
layout: post
title:  "Find the missing number in an array"
date:   2020-10-05
excerpt: "The good and the bad one"
tags: [kata, software, code, array, time complexity]
comments: true
---
The key of being better at something is exercise. Code kata is an exercise in programming which helps programmers hone their skills through practice and repetition.

I decided to share some katas I did with short explanation why they are good or not.

***Code kata:***
Given an array C of size N-1 and given that there are numbers from 1 to N with one element missing, the missing number is to be found.

***Input:***
The first line of input contains an integer T denoting the number of test cases. For each test case first line contains N(size of array). The subsequent line contains N-1 array elements.

***Output:***
Print the missing number in array.


One of mine not so great solutions is the one written in C# where I'm searching for the missing number by looping through the array from 1 to N and trying to find those values in a given array.
When the certain number isn't found, that's the missing number!

Problem with this solution is that looping through 1 to N has O(N) time complexity and Array.Find() has also O(N) complexity,
that gives us O(N^2) time complexity in total.

```
using System;
public class GFG {
	static public void Main () {
	    int testCases;
	    int counter = 0;
	    testCases = Int32.Parse(Console.ReadLine());

	    while(counter < testCases) {
    	    int length;
            length = Int32.Parse(Console.ReadLine());

            int[] arr = new int[length];
            string[] s = Console.ReadLine().Split(' ');

            for(int i= 0;i< s.Length;i++)
             {
				try {
					arr[i] = Int32.Parse(s[i]);
				}
				catch{
					if(length == 1){
					 arr[i] = 0;
					}
					else {
					Console.WriteLine("invalid input");
					return;
					}
				};

             }

    		int result = MissingNumber(length, arr);
    		Console.WriteLine(result);
    		counter++;
        }

	}

  static int MissingNumber(int length, int[] array) {
     int missNum = 0;

     for(int i= 1;i < length + 1;i++)
      {
        int result = Array.Find(array, element => element == i);

        if(result == 0){
          missNum = i;
          break;
        }
      }

      return missNum;
    }
}
```
The better solution is one written in Ruby where the missing number is the difference between sum of N number and the given array. This way is much nicer and simpler - time complexity O(1).

```
def missing_number(length, array)
  sum_of_length_numbers = length * (length + 1) /2
  sum_of_array = array.sum

  sum_of_length_numbers - sum_of_array
end
```
