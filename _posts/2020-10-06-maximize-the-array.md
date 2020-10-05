---
layout: post
title:  "Maximize the array"
date:   2020-10-05
excerpt: "Maximize the first array by using the elements from the second array"
tags: [kata, software, code, array, time complexity]
comments: true
---
You are given two arrays of numbers. You have to maximize the first array by using the elements from the second array such that the new array formed contains unique elements. For example, let the size of array be 'n'. Then the output should be n greatest but unique elements of both the arrays. The order of elements should be as explained in example below, i.e., giving the **second array priority**.

**Input:**
The first line of each input contains the number of test cases . The description of T test cases follows:
The first line of each test case contains the size of the array (This is going to be the size of both the arrays). The second line of each test case contains the elements of the first array.The final line of each test case contains the elements of the second array.

**Output:**
Print the maximum elements giving second array higher priority. The order of appearance of elements is kept same in output as in input (See Example for better Understanding).

**Constraints:**

1 <= T <= 20

1 <= N <= 10

0 <= Array 1 [i] <= 9

0 <= Array 2 [i] <= 9


**Example:**

*Input*:

2

5

7 4 8 0 1

9 7 2 3 6

4

6 7 5 3

5 6 2 9

*Output:*

9 7 6 4 8

5 6 9 7



I'm using union of two arrays to get unique values in one single array. Then I'm sorting and reversing the array to start from the highest number. After selecting first N (where N is length of input arrays), the output must be in the same order as the order of numbers in original arrays. To support that, Intersect (&) is applied.
The solutions below are in C# and Ruby.
```
using System;
using System.Linq;
public class GFG {
	static public void Main () {
		int testCases;
		int counter = 0;
		testCases = Int32.Parse(Console.ReadLine());

		while(counter < testCases) {
			int length;
			length = Int32.Parse(Console.ReadLine());

			int[] arr1 = new int[length];
			int[] arr2 = new int[length];


			string[] s1 = Console.ReadLine().Split(' ');
			string[] s2 = Console.ReadLine().Split(' ');

			for(int i= 0;i< s1.Length;i++)
			{
				try{
					arr1[i] = Int32.Parse(s1[i]);
				}
				catch {

				};

			}

			for(int i= 0;i< s2.Length;i++)
			{
				try {
					arr2[i] = Int32.Parse(s2[i]);
				}
				catch {
				};

			}

			var result = MaxArray(arr1, arr2);
			for(int i= 0;i < result.Length;i++) {
				Console.Write(result[i] + " ");
			};
			Console.WriteLine("");

			counter++;
		}
	}

	static public int[] MaxArray(int[] arr1, int[] arr2) {
		var arr = arr1.Union(arr2).ToArray();

		Array.Sort(arr);
		Array.Reverse(arr);

		var firstLengthItems = arr.Take(arr1.Length).ToArray();
		var first = arr2.Intersect(firstLengthItems);
		var second = arr1.Intersect(firstLengthItems);
		var final = first.Union(second).ToArray();

		return final;

	}
}
```
Partial solution using Ruby:

```
def max_numbers(array1, array2)
 max_numbers = [array2, array1].flatten.uniq.sort.reverse.first(5)

 first = array2.select { |a| max_numbers.include?(a)}
 second = array1.select { |a| max_numbers.include?(a)}

 [first, second].flatten.uniq
end
```
