# ds
recursion
package algs11;

import stdlib.StdDraw;
import stdlib.StdOut;
import stdlib.StdRandom;
import stdlib.Trace;

import java.util.Arrays;

public class MyRecursion {

	/** iterative version */
	public static double minValueI(double[] list) {
		double result = list[0];
		int i = 1;
		while (i < list.length) {
			if (list[i] < result) result = list[i];
			i = i + 1;
		}
		return result;
	}

	/** recursive version */
	public static double minValue(double[] list) {
		return minValueHelper(list, 1, list[0]);
	}
	private static double minValueHelper(double[] list, int i, double result) {
		if (i < list.length) {
			if (list[i] < result) result = list[i];
			result = minValueHelper(list, i + 1, result);
		}
		return result;
	}

	public static double sum(double[] a) {
		return sumHelper(a, 0);
	}
	private static double sumHelper(double[] a, int i) {
		if (i == a.length) return 0.0;
		return a[i] + sumHelper(a, i + 1);
	}

	public static void reverse(double[] a) {
		reverseHelper(a, 0, a.length - 1);
	}
	private static void reverseHelper(double[] a, int lo, int hi) {
		if (lo >= hi) return;
		double temp = a[lo];
		a[lo] = a[hi];
		a[hi] = temp;
		reverseHelper(a, lo + 1, hi - 1);
	}

	public static void draw(double centerX, double centerY, double radius) {
		if (radius < .0005) return;
		StdDraw.setPenColor(StdDraw.LIGHT_GRAY);
		StdDraw.filledCircle(centerX, centerY, radius);
		StdDraw.setPenColor(StdDraw.BLACK);
		StdDraw.circle(centerX, centerY, radius);
		double change = radius * 0.90;
		draw(centerX + change, centerY + change, radius / 2);
		draw(centerX - change, centerY + change, radius / 2);
	}

	


	public static void runTerribleLoop() {
		for (int N = 0; N < 100; N++)
			StdOut.format("terribleFibonacci(%2d)=%d\n", N, terribleFibonacci(N));
	}
	public static long terribleFibonacci(int n) {
		if (n <= 1) return n;
		return terribleFibonacci(n - 1) + terribleFibonacci(n - 2);
	}

	public static long fibonacci(int n) {
		if (n <= 1) return n;
		long prev = 0, curr = 1;
		for (int i = 2; i <= n; i++) {
			long next = prev + curr;
			prev = curr;
			curr = next;
		}
		return curr;
	}

	public static void runFibonacciLoop() {
		for (int N = 0; N < 100; N++)
			StdOut.format("fibonacci(%2d)=%d\n", N, fibonacci(N));
	}

	public static void main(String[] args) {
		testSum("11 21 81 -41 51 61");
		testSum("11 21 81 -41 51");
		testSum("11 21 81 -41");
		testSum("11 21 81");
		testSum("11 21");
		testSum("11");
		testSum("");

		testReverse("11 21 81 -41 51 61");
		testReverse("11 21 81 -41 51");
		testReverse("11 21 81 -41");
		testReverse("11 21 81");
		testReverse("11 21");
		testReverse("11");
		testReverse("");

		testFibonacci(0, 0);
		testFibonacci(1, 1);
		testFibonacci(1, 2);
		testFibonacci(2, 3);
		testFibonacci(21, 8);
		testFibonacci(233, 13);
		testFibonacci(75025, 25);
		testFibonacci(1836311903L, 46);
		testFibonacci(2971215073L, 47);
		testFibonacci(308061521170129L, 71);
		testFibonacci(498454011879264L, 72);
		testFibonacci(7540113804746346429L, 92);
		testFibonacci(-6246583658587674878L, 93);
		testFibonacci(-813251414217914645L, 376);
		StdOut.println("Finished tests");

		draw(.5, .5, .25);

		// TODO: uncomment these temporarily when you want to see the output of your Fibonacci functions
		// runTerribleLoop();
		// runFibonacciLoop();
	}

	public static double sumI(double[] a) {
		double result = 0.0;
		int i = 0;
		while (i < a.length) {
			result = result + a[i];
			i = i + 1;
		}
		return result;
	}
	public static void reverseI(double[] a) {
		int hi = a.length - 1;
		int lo = 0;
		while (lo < hi) {
			double loVal = a[lo];
			double hiVal = a[hi];
			a[hi] = loVal;
			a[lo] = hiVal;
			lo = lo + 1;
			hi = hi - 1;
		}
	}

	private static void testSum(String list) {
		double[] aList = doublesFromString(list);
		double expected = sumI(aList);
		double actual = sum(aList);
		if (!Arrays.equals(aList, doublesFromString(list))) {
			StdOut.format("Failed sum([%s]): Array modified\n", list);
		}
		if (expected != actual) {
			StdOut.format("Failed sum([%s]): Expecting (%.1f) Actual (%.1f)\n", list, expected, actual);
		}
	}
	private static void testReverse(String list) {
		double[] expected = doublesFromString(list);
		reverseI(expected);
		double[] actual = doublesFromString(list);
		reverse(actual);
		if (!Arrays.equals(expected, actual)) {
			StdOut.format("Failed reverse([%s]): Expecting (%s) Actual (%s)\n", list,
					Arrays.toString(expected), Arrays.toString(actual));
		}
	}
	private static void testFibonacci(long expected, int n) {
		long actual = fibonacci(n);
		if (expected != actual) {
			StdOut.format("Failed fibonacci(%d): Expecting (%d) Actual (%d)\n", n, expected, actual);
		}
	}
	private static double[] doublesFromString(String s) {
		if ("".equals(s)) return new double[0];
		String[] nums = s.split(" ");
		double[] result = new double[nums.length];
		for (int i = nums.length - 1; i >= 0; i--) {
			result[i] = Double.parseDouble(nums[i]);
		}
		return result;
	}
}
