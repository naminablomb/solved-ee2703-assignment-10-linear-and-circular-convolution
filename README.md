Download Link: https://assignmentchef.com/product/solved-ee2703-assignment-10-linear-and-circular-convolution
<br>
One of the main uses of the DFT is to implement Convolution. As you know from your course in DSP, the convolution is defined as

<em>y</em>                                                      (1)

<table width="480">

 <tbody>

  <tr>

   <td width="465"><em>k</em>=−∞In fourier space, this becomes</td>

   <td width="15"> </td>

  </tr>

  <tr>

   <td width="465"><em>Y</em>[<em>m</em>] = <em>X</em>[<em>m</em>]<em>H</em>[<em>m</em>]</td>

   <td width="15">(2)</td>

  </tr>

 </tbody>

</table>

which is a major simplification. We will only look at convolution of causal signals, so the summation in Eq. 1 is from <em>k </em>= 0 to <em>k </em>= ∞.

<h1>Linear Convolution using Direct Summation</h1>

The summation in Eq. 1 can be implemented directly. We assume that the filter, <em>h</em>[<em>k</em>], has coefficients from <em>k </em>= 0 to <em>k </em>= <em>N </em>−1. So we can have an algorithm that implements Eq. 1 as follows:

for n corresponding to x: # iterate over indices in x y[n]=0;            # initialize y to zero

for k in range(K):                                      # iterate over indices in h

y[n] += x[n-k]*h[k]                          # compute the convolution sum

#end for

#end for

We initialize <em>y </em>to zero since the sum started at <em>k </em>= 0. If <em>y </em>had any initial value, it would have decayed by the time we reached <em>t </em>= <em>nT</em>.

<h2>Computational Cost</h2>

How many computations are required? One multiplication and one addition per index in <em>h</em>.

So the total cost is

cost = <em>N </em>×<em>K </em>multiplications and <em>N </em>×<em>K </em>additions

Typical FIR filters have 50 taps, and so the cost can be quite prohibitive.

In python, how would you optimize this computation? Can you eliminate one or both for loops?

There is a library function, np.convolve, that implements this operation.

<h2>Start and End of sequence</h2>

The above algorithm is fine for an infinite sequence <em>x</em>[<em>n</em>]. What happens if <em>x</em>[<em>n</em>] was a finite sequence? Let us assume that <em>x </em>is present from <em>n</em><sub>1 </sub>to <em>n</em><sub>2</sub>, <em>n</em><sub>1 </sub>≤ <em>n</em><sub>2</sub>

<ul>

 <li>Consider any <em>n</em>. Then,</li>

</ul>

<em>x</em>[<em>n</em>−<em>k</em>]∗<em>h</em>[<em>k</em>]

has terms for 0 ≤ <em>k </em><em>&lt; N </em>and for <em>n</em><sub>1</sub>≤ <em>n</em>−<em>k </em>≤ <em>n</em><sub>2</sub>, i.e., for 0 ≤ <em>k </em><em>&lt; N </em>and <em>n</em><sub>1</sub>+<em>k </em>≤ <em>n </em>≤ <em>n</em><sub>2</sub>+<em>k</em>. Since we assume <em>k </em>to be non-negative, the summation has terms for

<em>n </em>≥ <em>n</em><sub>1 </sub>and <em>n </em>≤ <em>n</em><sub>2</sub>+<em>N </em>−1

<ul>

 <li>For which <em>n </em>does <em>y</em>[<em>n</em>] use the full set of terms in the summation? Clearly this is for <em>n</em><sub>1</sub>+<em>N </em>−1 ≤ <em>n </em>≤ <em>n</em><sub>2</sub></li>

 <li>If we pad <em>x</em>[<em>n</em>] with zeros on both sides, the original algorithm will continue to work (though with some unnecessary additional calculations)</li>

</ul>

<h1>Circular Convolution using the DFT</h1>

You will have learnt that the DFT also supports a convolution. This is defined by (using Oppenheim’s notation)

<em>x</em>˜[<em>n</em>] = <sup>1 </sup><em>N</em>∑−1<em>X</em>˜[<em>k</em>]<em>e</em><em><sub>j</sub></em>(2<em>π</em><em>/</em><em>N</em>)<em>kn</em>

<em>N <sub>k</sub></em><sub>=0</sub>

<em>N</em>−1

<em>X</em>˜[<em>k</em>] = ∑ <em>n</em>˜[<em>n</em>]<em>e</em>−<em>j</em>(2<em>π</em><em>/</em><em>N</em>)<em>kn n</em>=0

where the˜in ˜<em>x </em>means the sequence of <em>N </em>values has been extended into an infinite periodic sequence, so that

0 ≤ <em>n </em><em>&lt; N x</em>˜<em>N </em>≤ <em>n </em><em>&lt; </em>2<em>N</em>

<sup></sup><em>x</em>[<em>n</em>+<em>N</em>] −<em>N </em>≤ <em>n </em><em>&lt; </em>0

The filter <em>h</em>[<em>k</em>] does not need to be extended, as it is used without shifting. But we can use <em>h</em>˜[<em>k</em>] without any harm. Then, the convolution of <em>x</em>[<em>n</em>] and <em>h</em>[<em>k</em>] is given by

<em>N</em>−1 <em>y</em>˜[<em>n</em>] = ∑ <em>x</em>˜[<em>n</em>−<em>m</em>]<em>h</em>˜[<em>m</em>] <em>m</em>=0 <em>N</em>−1

<table width="480">

 <tbody>

  <tr>

   <td width="465">= ∑ <em>x</em>[(<em>n</em>−<em>m</em>)modulo<em>N</em>]<em>h</em>[<em>m</em>] <em>m</em>=0Taking the DFT, this becomes</td>

   <td width="15">(3)</td>

  </tr>

  <tr>

   <td width="465"><em>Y</em>˜[<em>k</em>] = <em>X</em>˜[<em>k</em>]<em>H</em>˜[<em>k</em>]</td>

   <td width="15">(4)</td>

  </tr>

 </tbody>

</table>

How long a sequence is needed for <em>Y</em>˜ to be correct? The first window is moved over the second (according to our definition. Oppenheim uses the opposite convention and moves the second window over the first). The length of the resulting sequence is the sum of the two lengths less by one. For the above case, it is 2<em>N </em>−1.

What if length of ˜<em>x </em>plus length of <em>h</em>˜ is larger than <em>N </em>+1?

While this is nice, it is not very practical, since we rarely want to convolve two periodic sequences. What we do have are filters with finite impulse responses (FIR filters) and infinite sequences to be filtered by these filters. That is covered in the next section.

<h2>Computational Speed</h2>

Equation 3 costs <em>N</em><sup>2 </sup>multiplications and a similar number of addtions. But Eq. 4 Costs <em>N</em>log<sub>2</sub><em>N </em>to get the DFT of <em>x</em>[<em>n</em>], another <em>N</em>log<sub>2</sub><em>N </em>to convert back to <em>y</em>[<em>n</em>] (We must do the same for <em>h</em>[<em>k</em>] but this has to be done only once). Finally we need <em>N </em>operations to compute the convolution in frequency domain. The total cost is only 2<em>N</em>log<sub>2</sub><em>N </em>+<em>N </em>multiplications and additions, which is much less than the <em>N</em><sup>2 </sup>cost for Eq. 3.

<h1>Using the DFT to implement Linear Convolution</h1>

Suppose now we have a low pass filter. We want to apply it to a speech signal. The speech signal is very long (say 5 minutes worth at 44 Ksmp/sec, or 13<em>,</em>200<em>,</em>000 samples). The filter is an FIR filter, with 32 taps.

If we do linear convolution this is going to cost us 1<em>.</em>32×10<sup>7</sup>×32 operations, or 422 million operations in all.

If somehow we could do this using FFTs, we could do it in less than 150 million operations.

But how to do it?

<h2>Circular Convolution as Linear Convolution with Aliasing</h2>

<ul>

 <li>Suppose <em>h</em>[] fits into a 2<em><sup>m </sup></em> We zero pad the <em>h</em>[<em>k</em>] if required.</li>

 <li>Divide <em>x</em>[] into sections 2<em><sup>m </sup></em></li>

 <li>Apply circular convolution to each section of <em>x</em>[].</li>

 <li>Stitch the outputs back together.</li>

</ul>

The problem with this approach is that each convolution is longer than the section of <em>x</em>[]. So, we actually have to have <em>x</em>[] sliced into smaller sections so that if <em>L </em>is the length of the <em>x</em>[] sections, and <em>P </em>is the length of <em>h</em>[], then <em>N </em>= <em>L</em>+<em>P</em>−1.

This now gives us <em>N </em>samples of <em>y</em>[], the samples corresponding to the convolution of <em>h</em>[] with a section of <em>x</em>[] that is <em>L </em>samples in size. So which of these samples would agree with the <em>y</em>[] obtained from linear convolution?

The linear convolution corresponds to moving <em>h</em>[] over <em>x</em>[] and summing the portion that overlaps. i.e.,

<em>P</em>−1 <em>y</em>[<em>n</em>] = ∑ <em>x</em>[<em>n</em>−<em>k</em>]<em>h</em>[<em>k</em>]

<em>k</em>=0

So the first <em>P</em>−1 samples of <em>y</em>[] are incorrect since some of the <em>x</em>[] values don’t exist in the slice we used. Similarly the last <em>P</em>−1 samples are also wrong for the same reason (they need future <em>x</em>[<em>n</em>]). The middle samples are correct.

So what can we do? We first save the last <em>P</em>−1 samples (call them <em>w</em>[0] to <em>w</em>[<em>P</em>−2]) of the previous convolution. The current slice (of length <em>L </em>= <em>N </em>+1−<em>P</em>) is now convolved with <em>h</em>[].

There are <em>P</em>−1 zeros prior to the first <em>x</em>[<em>n</em>] value in the slice. In the convolution, these do not correspond to any <em>x</em>[<em>n</em>], since we have <em>L </em>values per slice of <em>x</em>[] and have <em>P</em>−1 extra places that are loaded with zeros. Call the output of the circular convolution for these places as <em>u</em>[0] to <em>u</em>[<em>P</em>−2].

<em>Then, the last P</em>−1 <em>values of the previous convolution are given by</em>

<em>y</em>[<em>m</em>+(<em>L</em>−<em>P</em>+1)+<em>k</em>] = <em>w</em>[<em>k</em>]+<em>u</em>[<em>k</em>]

Following these, we have <em>L</em>−<em>P </em>correct <em>y </em>values. And finally we have <em>P</em>−1 more entries that are incorrect, waiting for the next circular convolution to correct them.

In each circular convolution, we use <em>L </em>new <em>x </em>values, and get <em>L </em>new <em>y </em>values.

The best way to understand this is to draw diagrams which will be done in the Lab Lecture. (You can also go through the DFT chapter of Oppenheim and Schafer, though that is a little obscure).

<h2>Computational Cost</h2>

We do one <em>N</em>-point DFT, <em>N </em>multiplications and additions, and an <em>N</em>-point inverse DFT. After that we add <em>w </em>and <em>u </em>values (<em>P</em>−1 of them). So the total computational cost (only counting multiplications) is

cost to get <em>L </em>values = 2<em>N</em>log<sub>2</sub><em>N </em>+<em>N </em>+(<em>P</em>−1)

Direct linear convolution of <em>L </em>samples is <em>L</em>×<em>P </em>= (<em>N </em>−<em>P</em>+1)×<em>P</em>

As long as 2log<sub>2</sub><em>N </em>+1 <em>&lt; P </em>the circular convolution is faster than linear convolution.

<h1>Assignment</h1>

<ol>

 <li>Download h.csv from Moodle. The file contains coefficients for an FIR filter.</li>

 <li>Plot the magnitude and phase response of the filter and infer the properties of thefilter.</li>

</ol>

Read up scipy.signal.freqz() to plot the magnitude and phase response of a digital filter.

<ol start="3">

 <li>Generate the following sequence and plot:</li>

</ol>

<em>x </em>= <em>cos</em>(0<em>.</em>2∗ <em>pi</em>∗<em>n</em>)+<em>cos</em>(0<em>.</em>85∗ <em>pi</em>∗<em>n</em>)                                      (5)

Where n is 1, 2, 3, … 2<sup>10</sup>

<ol start="4">

 <li>Pass the sequence x through the filter using linear convolution and plot the output.</li>

</ol>

What do you observe?

<ol start="5">

 <li>Now repeat process using circular convolution. Plot the output.What is difference do you observe?</li>

</ol>

Note that computational cost of circular convolution is less in frequency domain.

Use the DFT technique discussed to do the convolution. Note: You will have to zero-pad the filter approprietly.

y1=np.fft.ifft(np.fft.fft(x) * np.fft.fft( np.concate(( h, np.zeros(1,len(x)-len(h)) )))

<ol start="6">

 <li>Now, do the linear convolution using circular convolution.Refer page 3.</li>

 <li>Circular Correlation</li>

</ol>

In this section, we will take an example sequence which is widely used in communications called as Zadoff-Chu sequences. They are named after Solomon A.Zadoff and D.C.Chu.

Read the Zadoff–Chu sequence from x1.csv. Properties of Zadoff-Chu sequence:

<ul>

 <li>It is a complex sequence.</li>

 <li>It is a constant amplitude sequence.</li>

 <li>The auto correlation of a Zadoff–Chu sequence with a cyclically shifted versionof itself is zero.</li>

 <li>Correlation of Zadoff–Chu sequence with the delayed version of itself will givea peak at that delay.</li>

</ul>

Correlation can be written as conv(a(n),b(-n)) which in frequency domain is A(jw).*conj(B(jw)).

Correlate x1 with a cyclic shifted version of x1, with a shift of 5 to the right. Plot the correlation output. Note that this is a complex sequence hence you need to plot the magnitude.

What do you observe?