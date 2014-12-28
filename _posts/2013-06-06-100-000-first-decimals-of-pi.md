---
title: 100 000 first decimals of Pi
layout: post
permalink: /2013/06/06/100-000-first-decimals-of-pi/
tags: [maths, java]
---
Did you ever wonder how many decimals of Pi your computer could compute ? Well using one ([of many][1]) algorithms to approximate Pi, I got a surprising answer with the following java code.

## Code

{% highlight java linenos %}
import java.math.BigDecimal;
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.BrokenBarrierException;
/**
*
* @author onnetz
*/
public class ArctanPi {

final BigDecimal FOUR = new BigDecimal("4");
BigDecimal arctan1_5, arctan1_239, pi;

final int roundingMode = BigDecimal.ROUND_HALF_EVEN;
final int digits;
Thread[] threads;
int numProcessors;
CyclicBarrier cb;

public ArctanPi(int digits) {
this.digits = digits;
numProcessors = Runtime.getRuntime().availableProcessors();
// numProcessors = 2;
}

public void startPi() {
threads = new Thread[numProcessors];
cb = new CyclicBarrier(numProcessors+1, new Runnable() {
public void run() {
finishUp();
}
});

for(int a = 0; a &lt; numProcessors; a++) {
threads[a] = new ArctanPi.MyThreads();
threads[a].start();
}
try {
cb.await();
} catch(InterruptedException ie) {

} catch(BrokenBarrierException be) {

}
}

/**
* Compute the value of pi to the specified number of
* digits after the decimal point. The value is
* computed using Machin's formula:
*
* pi/4 = 4*arctan(1/5) - arctan(1/239)
*
* and a power series expansion of arctan(x) to
* sufficient precision.
*/
public void computePi() {
try {
arctan1_5 = arctan(5, digits+5);
arctan1_239 = arctan(239, digits+5);

cb.await();
} catch(InterruptedException ie) {
ie.printStackTrace();
} catch(BrokenBarrierException be) {
be.printStackTrace();
}

}

public void finishUp() {
pi = arctan1_5.multiply(FOUR).subtract(
arctan1_239).multiply(FOUR);
pi.setScale(digits,
BigDecimal.ROUND_HALF_UP);
}

/**
* Compute the value, in radians, of the arctangent of
* the inverse of the supplied integer to the specified
* number of digits after the decimal point. The value
* is computed using the power series expansion for the
* arc tangent:
*
* arctan(x) = x - (x^3)/3 + (x^5)/5 - (x^7)/7 +
* (x^9)/9 ...
*/
public BigDecimal arctan(int inverseX,
int scale)
{
BigDecimal result, numer, term;
BigDecimal invX = BigDecimal.valueOf(inverseX);
BigDecimal invX2 =
BigDecimal.valueOf(inverseX * inverseX);

numer = BigDecimal.ONE.divide(invX,
scale, roundingMode);

result = numer;
int i = 1;
do {
numer =
numer.divide(invX2, scale, roundingMode);
int denom = 2 * i + 1;
term =
numer.divide(BigDecimal.valueOf(denom),
scale, roundingMode);
if ((i % 2) != 0) {
result = result.subtract(term);
} else {
result = result.add(term);
}
i++;
} while (term.compareTo(BigDecimal.ZERO) != 0);
return result;
}

/**
* @param args the command line arguments
*/
public static void main(String[] args) {
int digits = 100000;
ArctanPi m = new ArctanPi(digits);
long stime = System.currentTimeMillis();
m.startPi();
long etime = System.currentTimeMillis();
System.out.println((double)(etime - stime)/1000 +
" secs for " + digits + " digits.");
}

class MyThreads extends Thread {
public void run() {
computePi();
}
}

}
{% endhighlight %}

##Compilation

{% highlight bash %}
$ javac ArctanPi.java
{% endhighlight %}

##Running

{% highlight bash %}
$ java ArctanPi
{% endhighlight %}

##Results

| Nb of <br />processor | result |
|:----:|:----|
| 1 | 20.717 secs for 100.000 digits. | 
| 2 | 20.458 secs for 100.000 digits. |
| 3 | 25.696 secs for 100.000 digits. |
| 4 | 27.938 secs for 100.000 digits. |

More processor doesn't not necessarily mean better results.

[source](http://www.overclock.net/t/737822/java-calculate-pi-multithreaded)

 [1]: http://en.wikipedia.org/wiki/Numerical_approximations_of_%CF%80