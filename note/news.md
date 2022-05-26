# Further research on APCI

Time delay interferometry (TDI) is a post-processing algorithm for suppressing the laser frequency noise (LFN). 

Space-based gravitational-wave detector data include the outputs of several interferometers, which can be expressed in relative frequency deviations. We denote the corresponding measurements as $s_{ij}(t)$ to conform to the official convention, where *i* is the index of the
satellite hosting the optical bench, and *j* is the index of the sending spacecraft. In practice, we measure a discrete version of $s_{ij}$ with *N* samples spaced every $\tau_s$ seconds. Therefore, we can form a vector $s_{ij}(t)$ with *N* entries given by $s_{ij}(t_n)$, where the timestamps are $t_n=n\tau_s$, $n ∈[0,N-1]$. Hence, we have $\boldsymbol{s}_{ij}=(s_{ij}(t_0), ..., s_{ij}(t_{N-1}))^\intercal$. We will stick to the LISA example, where the detector is made of a constellation of three satellites, each of them carrying two optical benches on board. If we consider the science measurements only, then we have six measurements that we can stack into an *N × 6* matrix ***Y*** as  
$$
\boldsymbol{Y} \equiv\left(\boldsymbol{s}_{12}, \boldsymbol{s}_{23}, \boldsymbol{s}_{31}, \boldsymbol{s}_{13}, \boldsymbol{s}_{21}, \boldsymbol{s}_{32}\right)
$$
<img src="D:\lhsProgrammes\Projects_2022\TDI\image\image-20220526221002652.png" alt="image-20220526221002652" style="zoom:67%;" />

Classical TDI algorithms operate in two steps. First, they compute **delayed** versions of discrete signals, interpolated at specific times depending on the light travel time delays along the constellation arms. In a second step, they combine these interpolated time series in such a way that laser noise terms vanish.  

- Fractional-delay filter (interpolation) can be performed by convolution as follows.
    $$
    s_{ij}(n-D)=s_{ij}(n)*v_{ij}(n-D)
    $$
    Rewrite it as the matrix form,
    $$
    s_{ij}^\prime=s_{ij}*v_{ij}=\boldsymbol{Q}_{ij} \boldsymbol{v}_{ij}\\ = \left[ \begin{matrix}
    	s_{ij}\left( 0 \right)&		0&		\cdots&		0&		0\\
    	s_{ij}\left( 1 \right)&		s_{ij}\left( 0 \right)&		&		\vdots&		\vdots\\
    	s_{ij}\left( 2 \right)&		s_{ij}\left( 1 \right)&		\cdots&		0&		0\\
    	\vdots&		s_{ij}\left( 2 \right)&		\cdots&		s_{ij}\left( 0 \right)&		0\\
    	s_{ij}\left( 2n_h \right)&		\vdots&		\ddots&		s_{ij}\left( 1 \right)&		s_{ij}\left( 0 \right)\\
    	s_{ij}\left( 2n_h+1 \right)&		s_{ij}\left( 2n_h \right)&		&		\vdots&		s_{ij}\left( 1 \right)\\
    	\vdots&		\vdots&		\ddots&		\vdots&		\vdots\\
    	0&		0&		\cdots&		s_{ij}\left( n-1 \right)&		s_{ij}\left( n-2 \right)\\
    	\vdots&		\vdots&		&		s_{ij}\left( n \right)&		s_{ij}\left( n-1 \right)\\
    	0&		0&		0&		\cdots&		s_{ij}\left( n \right)\\
    \end{matrix} \right] \left[ \begin{array}{c}
    	v_{ij}\left( -n_h \right)\\
    	v_{ij}\left( -n_h+1 \right)\\
    	v_{ij}\left( -n_h+2 \right)\\
    	\vdots\\
    	v_{ij}\left( n_h \right)\\
    \end{array} \right]
    $$
    where $\boldsymbol{Q}_{ij}$ is the Toeplitz matrix for the $j \rightarrow i$ channel.

- TDI combinations can be represented as 
    $$
    \boldsymbol{TDI}_{N\times m}=\boldsymbol{X}_{N\times 6(2n_h+1)}\boldsymbol{V}_{6(2n_h+1)\times m}\\=\left[ \begin{matrix}
    	\boldsymbol{D}_{-n_h}\boldsymbol{Y}&		\boldsymbol{D}_{-n_h+1}\boldsymbol{Y}&		...&		\boldsymbol{D}_{-n_h}\boldsymbol{Y}\\
    \end{matrix} \right]\left[ \begin{matrix}
    	\boldsymbol{D}_{-n_h}\boldsymbol{v}^1&		\boldsymbol{D}_{-n_h}\boldsymbol{v}^2&		\cdots&		\boldsymbol{D}_{-n_h}\boldsymbol{v}^m\\
    	\boldsymbol{D}_{-n_h+1}\boldsymbol{v}^1&		\boldsymbol{D}_{-n_h+1}\boldsymbol{v}^2&		\cdots&		\boldsymbol{D}_{-n_h+1}\boldsymbol{v}^m\\
    	\vdots&		\vdots&		\ddots&		\vdots\\
    	\boldsymbol{D}_{n_h}\boldsymbol{v}^1&		\boldsymbol{D}_{n_h}\boldsymbol{v}^2&		\cdots&		\boldsymbol{D}_{n_h}\boldsymbol{v}^m\\
    \end{matrix} \right]\\
    $$
    where $\boldsymbol{v}^m=[v_{12}^m, v_{23}^m, v_{31}^m, v_{13}^m, v_{32}^m, v_{23}^m]^\intercal$​ and *m* stands for the *m*th TDI combination. The *m*th TDI is denoted as (following the terms of *Julia*)
    $$
    \boldsymbol{TDI}^m = (+)_{\cdot} (\boldsymbol{s}_{12}*\boldsymbol{v}_{12}^m, \boldsymbol{s}_{23}*\boldsymbol{v}_{23}^m,\boldsymbol{s}_{31}*\boldsymbol{v}_{31}^m,\boldsymbol{s}_{13}*\boldsymbol{v}_{13}^m,\boldsymbol{s}_{32}*\boldsymbol{v}_{32}^m,\boldsymbol{s}_{23}*\boldsymbol{v}_{23}^m)=\boldsymbol{X}\boldsymbol{V}_{:m}
    $$
    which can be transformed into the frequency domain.
    $$
    \widetilde{TDI}^m=(+)_.[@_\cdot(\boldsymbol{\tilde{s}}_{12}\times \boldsymbol{\tilde{v}}_{12}^{m},\boldsymbol{\tilde{s}}_{23}\times \boldsymbol{\tilde{v}}_{23}^{m},\boldsymbol{\tilde{s}}_{31}\times
    \boldsymbol{\tilde{v}}_{31}^{m},\boldsymbol{\tilde{s}}_{13}\times \boldsymbol{\tilde{v}}_{13}^{m},\boldsymbol{\tilde{s}}_{32}\times \boldsymbol{\tilde{v}}_{32}^{m},\boldsymbol{\tilde{s}}_{23}\times
    \boldsymbol{\tilde{v}}_{23}^{m})]\\
    $$
    

