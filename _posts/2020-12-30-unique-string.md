---
layout: post
title:  "Unique string"
date:   2020-12-30
excerpt: "Check does string contains unique characters"
tags: [string, software, code, array]
comments: true
---
Given a string, determine if the string has all unique characters.

```
using System;
using System.Collections.Generic;

public class ArraysAndStrings
{
	public bool IsUnique(string str){
		Dictionary<char, bool> hash = new Dictionary<char, bool>();

		for(int i = 0; i< str.Length; i++){
			if(hash.ContainsKey(str[i])){
				return false;
			}

			hash[str[i]] = true;

		}
        return true;
    }
	}
```
A solution using Ruby:

```
def unique?(str)
	arr = str.split('')

	arr.length == arr.uniq.length
end

```
