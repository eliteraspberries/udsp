Application programming interface
=================================

The udspy library includes functions for computing the FFT,
periodogram, convolution and correlation, and basic signal
processing.

The udsp library is thread-safe.


Data types
----------

Functions in the udsp library accept arguments of single precision
floating-point digits or arrays of them (corresponding to the
"binary32" type of the IEEE 754 standard);  array sizes are
indicated by a second parameter of type `size_t`.

The `udsp_complex_t` is defined by `udsp.h` as a structure with the
two members `real` and `imag`, both of type `float`.

Functions which need to store state information accept a pointer to
a structure called `udsp_state` as an argument.  This structure is
aliased as the `udsp_state_t` type.  This structure is a multiple
of 64 bits in size;  it is best allocated on the heap.

To initialize a `udsp_state` structure for an FFT computation, use
the function `udsp_fft_init`.  A single state structure can be
reused for multiple computations with inputs of the same length,
albeit not simultaneously.


Accuracy
--------

Functions in the udsp library return results accurate to about
ten units in the last place of single-precision floating-point
digits, that is, about 6 significant figures.


Spectral analysis
-----------------

These functions implement some common spectral analysis methods,
including the FFT, periodogram, convolution and correlation.

### Fast Fourier transform initialization

void **udsp_fft_init** ( udsp_state_t * *st* ,
    int *fft_method* , size_t *n* )

  Initialize a state structure pointed to by *st* for later use when
  computing an FFT of length *n* and using the method designated
  by *fft_method*.

  The value of *fft_method* determines which software method is used
  to compute the FFT:

  - UDSP_FFT_FFTPACK: FFTPACK routines by Paul N. Swarztrauber.

### Fast Fourier transform

void **udsp_fft** ( udsp_state_t * *st* ,
    float * *x* , size_t *n* , udsp_complex_t * *result* )

  Compute the fast Fourier transform of the real array *x* of
  length *n*, according to the array length and method decribed
  in the state structure pointed to by *st*, and store the complex
  transform coefficients in the array *result*.

  If the length *n* is less than the length indicated in the
  initialized state structure *st*, the array is first zero-padded
  to the latter length.  If *n* is greater than the length in *st*
  then the array is truncated.

### Inverse fast Fourier transform

void **udsp_ifft** ( udsp_state_t * *st* ,
    udsp_complex_t * *x* , size_t *n* , float * *result* )

  Compute the inverse fast Fourier transform of the array *x* of
  length *n*, according to the array length and method decribed in
  the state structure pointed to by *st*, and store the real output
  in the array *result*.

  If the length *n* differs from that in *st* the array is either
  zero-padded or truncated as described above.

### Circular shift

void **udsp_fft_shift** ( udsp_complex_t * *x* , size_t *n* )

  Shift the zero-frequency component of the complex array *x*
  to the centre index of the spectrum.

### Inverse circular shift

void **udsp_ifft_shift** ( udsp_complex_t * *x* , size_t *n* )

  Perform the inverse of the circular shift function above.

### Convolution

void **udsp_conv** ( udsp_state_t *st* [2],
    float * *x* , size_t *m* , float * *y* , size_t *n* ,
    float * *result* )

  Compute the convolution of two arrays *x* and *y* of lengths
  *m* and *n* respectively, and store the result in the array
  *result*.

  The array *result* must have a minimum length equal to the sum
  of the lengths of the input arrays, minus one.

### Cross-covariance

void **udsp_xcov** ( udsp_state_t *st* [2],
    float * *x* , size_t *m* , float * *y* , size_t *n* ,
    float * *result* )

  Compute the cross-covariance of two arrays *x* and *y* of lengths
  *m* and *n* respectively, and store the result in the array
  *result*.

  The array *result* must have a minimum length as described above.
  Its values are normalized by the larger of *m* or *n*.

  The lag values corresponding to the *result* array indices
  0, ..., *m* + *n* - 1 are -*n* + 1, ..., *m* - *n*.

### Cross-correlation

void **udsp_xcor** ( udsp_state_t *st* [2],
    float * *x* , size_t *m* , float * *y* , size_t *n* ,
    float * *result* )

  Compute the cross-correlation of two arrays *x* and *y* of
  lengths *m* and *n* respectively, and store the result in the
  array *result*.

  The array *result* and its corresponding lag values are as
  described above.

### Periodogram

void **udsp_pow** ( udsp_state_t * *st* ,
    float * *x* , size_t *n* , float * *result* )

  Compute the periodogram of the array *x* of length *n*, and store
  the result in the array *result* of the same length.

  The *result* array is normalized to 1.


Digital signal processing
-------------------------

...
