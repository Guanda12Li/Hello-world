debug的题目和我之前看得面经有点不一样，不过代码问题都是那几类。
1. distinctNumber 返回一个数组里distinct number的个数，好像是条件判断的时候!=写成了==
2. sumDistinct 返回数组里不一样的数字之和，代码会先对数组进行排序，要注意初始化sum数组应该在排序之后
3. eliminateVowel default condition里不需要i++
4. checkPalindrome 返回一个数字是不是回文。最后判断的时候把result==temp写成了result==num，但是num在中间已经被修改了
5. median 返回两个数组合并后的数组中位数。 访问第二个数组的时候下标越界，把i改成i-size
6. countDays 返回某一年某一月的天数，在判断闰年的时候条件有错 应该改成&&和==
7. elementRange 返回某个范围内的数。判断是否属于某个范围的时候，把&&写成了||

reasoning部分主要是字母找规律，圆桌问题，糖产量，admission问题，邮费



1. 检查一个function返回的数是基数还是偶数，num % 2 写反了，把！= 改成 == 就行。
2. 给一个数，把这个数字倒过来，比如给1234，返回4321. 其中reverseNum的计算错了，应该改成reverseNum = reverseNum * 10 + reminder.
3. 把一个string中的vowel都删掉，switch statement的default condition不需要i++，把那行删了就行。
4. 给两个array，把两个array合并成一个，这题会出现index out of bound exception，把arr = arr2 改成arr = arr2[i - size]。
5. 找出出现k的次数的element，要在loop后重新check一遍 if (count == k)
6. *给一个array和一个k，找出有多少数比k小，问题出在for loop的condition，把 i < k 改成 i < size就好。*
7. 给一个low和high的boundary，找出一串数字中有几个在这个range里面。把 || 改成 && 就行了。



两个array merge找中间数的： **index out of bound，把arr *= arr2 改成arr = arr2[i - size].***



check palindromic： *reverseNum的计算错了，应该改成reverseNum = reverseNum \* 10 + reminder.*





```java
package Amazon.OA1;

import java.util.Arrays;

public class Debugging {
	
	// 该函数对数组进行降序排列
	public static int[] sortArray(int arr[]) {
		int i, max, location, j, temp, len = arr.length;
		for(i = 0; i < len; i++) {
			max = arr[i];
			location = i;
			for(j = i; j < len; j++) {
				if(max < arr[j]) { // 原来是 max < arr[j]
					max = arr[j];
					location = j;
				}
			}
			temp = arr[i];
			arr[i] = arr[location];
			arr[location] = temp;
		}
		
		return arr;
		
	}
	
	// 降序排列
	public static int[] sortArray2(int arr[]) {
		int len = arr.length;
		int small, pos, i, j, temp;
		for(i = 0; i <= len - 1; i++) {
			for(j = i; j < len; j++) {
				temp = 0;
				if(arr[i] < arr[j]) { // 原先是arr[i] > arr[j]
					temp = arr[i];
					arr[i] = arr[j];
					arr[j] = temp;
				}
			}
		}
		return arr;
	}
	public static int countDigits(int num) {
		int count = 0;
		int temp = num;
		while(temp != 0) {
			temp /= 10;
			count++;
		}
		
		return num % count;
	}
	
	public static int[] replaceValues(int arr[]) {
		int i, j, len = arr.length;
		if(len % 2 == 0) {
			for(i = 0; i < len; i++) { // 原来是 i <= len
				arr[i] = 0;
			}
		}else {
			for(j = 0; j < len; j++) { // 原来是 j <= len
				arr[j] = 1;
			}
		}
		return arr;
	}
	public static int[] reverseArray(int arr[]) {
		int i, temp, originalLen = arr.length;
		int len = originalLen;
		for(i = 0; i < originalLen / 2; i++) {
//			System.out.println(arr[i] + ", " + arr[len - 1]);
			temp = arr[len - 1];
			arr[len - 1] = arr[i];
			arr[i] = temp;
			len -= 1; // 原来是 len += 1
		}
		return arr;
	}
	
	public static int[] removeElement(int arr[], int index) {
		int i, j, len = arr.length;
		if(index >= 0 && index < len) {
			for(i = index; i < len - 1; i++) {
				arr[i] = arr[i+1]; // 原来这里是arr[i] = arr[i++]
			}
			int rarr[] = new int[len-1];
			for(i = 0; i< len - 1; i++) {
				rarr[i] = arr[i];
				
			}
			return rarr;
		}else  
		return arr;		
	}
	
	public static void printPattern(int num) {
		int i, print = 0;
		if(num <= 0) return;
		if(num % 2 == 0) {
			print = 0;
			for(i = 0; i < num - 1;i++) {
				System.out.print(print + " ");
				print += 2;
			}
			System.out.println(print);
		}
		else {
			print = 1;
			for(i = 0; i < num - 1; i++) {
				System.out.print(print + " ");
				print += 2;
			}
			System.out.println(print);
		}
		
	}
	public static void printPattern2(int row) {
//		char ch = 'a'; // 错误代码里面ch在这里
		for(int i = 0; i < row; i++) {
			char ch = 'a';
			for(int j = 0; j<= i; j++) {
				System.out.print(ch++);
			}
			System.out.println("");
		}
		
	}
	public static int distinctElementsCount(int size, int[] elements) {
		int[] counted = new int[size];
		int count, flag;
		counted[0] = elements[0];
		count = 1;
		for(int i = 0; i < size; i++) {
			flag = 0;
			for(int j = 0; j < count; j++) {
				if(elements[i] == counted[j]) {
					flag = 1;
				}
				if(flag == 0) {
					System.out.println(Arrays.toString(counted) + " === " + elements[i]);
					count++;
					counted[count - 1] = elements[i];
					break;
				}
			}
		}
		return count;
	}
	public static int appearsKTimes(int size, int inputArray[], int k) {
		Arrays.sort(inputArray);// [1,2,2,3,3,4]
		int i = 1, count = 1;
		int element = inputArray[0];
		int res = -1;
		while(i < size) {
//			System.out.println(element);
			if(element == inputArray[i]) {
				count++;
			}
			else {
				System.out.println(count + " : " + element);
				if(count == k) {
					res = element;
//					System.out.println(element);
				}
				element = inputArray[i];
				count = 1;
//				System.out.println(i + " : "  + element);
			}
			i++;
		}
		if(count == k) res = element;
		
		return res;
	}
	public static int checkArmstrong(int num) {
		int digitCount = 0, result = 0;
		int temp = num;
		while(temp != 0) {
			temp = temp/10;
			digitCount++;
		}
		temp = num;
		while(temp != 0) {
			int remainder = temp % 10;
//			result += Math.pow(result, digitCount);
			result += Math.pow(remainder, digitCount);
			temp /= 10;
		}
		if(result == num) {
			return 1;
		}else {
			return 0;
		}
	}
	
	public static int sumDistinct(int size, int[] inputArray) {
//		int sum = inputArray[0]; // bug
		Arrays.sort(inputArray);
		int sum = inputArray[0];
		int point = inputArray[0];
		for(int i = 1; i < size; i++) {
			if (point != inputArray[i]) {
				sum += inputArray[i];
				point = inputArray[i];
			}
		}
		return sum;
	}
	
	public static String eliminateVowel(String str) {
		String newString = "";
		int i = 0;
		char[] S = str.toCharArray();
		int len = str.length();
		if(len == 0) return "";
		while(i < S.length) {
			switch(S[i]) {
//			default: 
////				System.out.println(i + " : " + S[i]);
//				newString += S[i];
//				i++;
				
			case 'a' : 
				i++;
				break;
			case 'e' : 
				i++;
				break;
			case 'i' : 
				i++;
				break;
			case 'o' : 
				i++;
				break;
			case 'u' : 
				i++;
				break;
			case 'A' : 
				i++;
				break;
			case 'E' : 
				i++;
				break;
			case 'I' : 
				i++;
				break;
			case 'O' : 
				i++;
				break;
			case 'U' : 
				i++;
				break;
			default: 
				newString += S[i];
				i++;
			}
		}
		return newString;
	}
	
	/**
	 * 
	 * matrixSum: 
	 * bug在于 sum += matrix[i++][j++]要改成sum += matrix[i][j++]
	 * 然后把i++放在外层循环
	 * 
	 */
	
	/**
	 * 
	 * countProduct: 
	 * bug在于 for循环里面循环的条件要改为j < size
	 * 
	 */
	
	/**
	 * 
	 * ElementCount: 
	 * bug在于 for循环里面循环的条件要改为j < len
	 * 
	 */
	/**
	 * 
	 * 那个merge两个array的错在index out of bound，把arr = arr2 改成arr = arr2[i - size].
	 * 那个palindrome的是因为reverseNum的计算错了，应该改成reverseNum = reverseNum * 10 + reminder.
	 * 找出出现k的次数的element那题要在loop后重新check一遍count == k
	 * 
	 */
	

	public static void main(String[] args) {
		// TODO Auto-generated method stub
//		int[] arr = {3,1,3,5,6};
//		int[] arr = {1,2,2,3,3,4};
//		int[] arr = {2,1,2};
		int[] arr = {5,2,1,2,2,3,3,4,4};
//		System.out.println(Arrays.toString(sortArray(arr)));
//		System.out.println(Arrays.toString(sortArray2(arr)));
//		System.out.println(countDigits(235));
//		System.out.println(Arrays.toString(replaceValues(arr)));
//		System.out.println(Arrays.toString(reverseArray(arr)));
//		System.out.println(Arrays.toString(removeElement(arr, 7)));
//		printPattern(4);
//		printPattern2(4);
//		System.out.println(distinctElementsCount(arr.length, arr));
//		System.out.println(appearsKTimes(7, arr, 2));
//		System.out.println(checkArmstrong(371));
//		System.out.println(sumDistinct(arr.length, arr));
		System.out.println(eliminateVowel("Happy"));

	}

}

```







```java
package Amazon.OA1;

public class DigitSum {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] arr = {2, 103, 35};
		System.out.println(getDigitSumParity(arr));

	}
	public  static int getDigitSumParity(int[] arr) {
		int min = getMin(arr);
		int result = getSum(min);
		if(result ==0) {
			return 0;
		}
		if(result % 2 == 0) { // bug 之前是 result % 2 != 0
			return 1;
		}else {
			return 0;
		}
			
	}

	private static int getSum(int num) {
		// TODO Auto-generated method stub
		int sum = 0;
		while(num != 0) {
//			num = num / 10; // bug
			int temp = num % 10;
			sum = sum + temp;
			num = num / 10;
		}
		return sum;
	}

	private static int getMin(int[] arr) {
		// TODO Auto-generated method stub
		if(arr == null || arr.length <= 0) {
			throw new IllegalArgumentException("NInput array should contain at least 1 element.");
		}
		int min = arr[0];
		for(int i = 1, len = arr.length; i< len; i++) {
			if(arr[i] < min) {
				min = arr[i];
			}
		}
		return min;
	}

}
```



