## Example of stochastic fields

Edit the module `stotemplate` to modify the features of the stochastic
fields required to implement the stochastic parameterization.
Of course, this could be provided through a parameter file.

All examples are produced over 200 timesteps,
for a 1° by 1° global grid on the sphere.

### Default stochastic field

A Gaussian white noise, with zero mean and unit standard deviation.
No specification required from the user.

File: `stofield_white.nc`

### Introduce time correlation

For instance with the specifications:

```
stofields(jsto)%type_t='arn'  ! Use autorgeressive processes
stofields(jsto)%nar_order=2   ! of order 2
stofields(jsto)%corr_t=5.0    ! with a correlation time scale of 5 time steps
stofields(jsto)%nar_update=5  ! with update every 5 time steps (and interpolation inbetween)
```

Other possibilities for `type_t` are `white` (default) and `constant`.
The cost is proportional to `nar_order` (default=1).
The cost decreases with `nar_update` (default=1), especially if the scheme to generate
the spatially correlated noise is expensive.
`nar_update` should not be larger than `corr_t`.

### Introduce space correlation (with diffusive operator)

For instance with the specifications (and keeping the time correlation specifications as above):

```
stofields(jsto)%type_xy='diffusive'  ! Use diffusive operator to obtain space correlation
stofields(jsto)%diff_passes=50       ! number of passes of the diffusion operator
stofields(jsto)%diff_type=0          ! type of diffusion operator (0=laplacian, default)
```

The cost is proportional to `diff_passes`, and may become quite large.
`diff_type=0` is the default and currently only available option.

### Introduce space correlation (with kernel convolution)

For instance with the specifications (and keeping the time correlation specifications as above):

```
stofields(jsto)%type_xy='kernel'     ! Use kernel convolution to obtain space correlation
stofields(jsto)%ker_type=0           ! type of kernel (0=gaussian, default)
stofields(jsto)%ker_coord=2          ! type of grid coordinates (2=spherical)
```

### Modify marginal distribution

### List of all possible options

Default value is given for each of the options.

```
      stofields(jsto)%stoname=default_name  ! Identifier, for instance: name=<physics>_<index>
      stofields(jsto)%type_t='white'        ! type of time structure (constant, white, arn)
      stofields(jsto)%type_xy='white'       ! type of xy structure (constant, white, diffusive, kernel,
      stofields(jsto)%type_z='constant'     ! type of z structure (constant, white)
      stofields(jsto)%type_variate='normal' ! type of marginal pdf(normal,wrapped_normal,lognormal,bounded_atan)
      stofields(jsto)%type_grid='T'         ! type of grid (T, U, V, F)
      stofields(jsto)%sign_grid=1._wp       ! sign to use to connect field across domain (1. or -1.)
      stofields(jsto)%ave=0._wp             ! average value, if homogeneous (default=0)
      stofields(jsto)%std=1._wp             ! standard deviation, if homogeneous (default=1)
      stofields(jsto)%min=0._wp             ! lower bound for bounded or cyclic distribution (default=none)
      stofields(jsto)%max=0._wp             ! upper bound for bounded or cyclic distribution (default=none)
      stofields(jsto)%corr_t=0._wp          ! correlation timescale, if homogeneous (default=0)
      stofields(jsto)%corr_xy=0._wp         ! correlation length, if homogeneous/isotropic (default=0)
      stofields(jsto)%corr_t_model='none'   ! type dependent, e.g. type of spectrum to use (not yet used)
      stofields(jsto)%corr_xy_model='none'  ! type dependent, e.g. which diffusive model
      stofields(jsto)%corr_xy_file='none'   ! file to read from (not yet used)
      stofields(jsto)%corr_z_file='none'    ! file to read from (not yet used)
      stofields(jsto)%corr_t_file='none'    ! file to read from (not yet used)
      stofields(jsto)%ave_file='none'       ! file to read from (not yet used)
      stofields(jsto)%std_file='none'       ! file to read from (not yet used)
      stofields(jsto)%nar_order=1           ! order of autoregressive processes (default=1)
      stofields(jsto)%nar_update=1          ! frequency of update of autoregressive processes (default=1)
      stofields(jsto)%diff_type=0           ! type of diffusion operator to use in 'diff' method (default=laplacian)
      stofields(jsto)%diff_passes=0         ! number of passes of the diffusive model (default=0)
      stofields(jsto)%ker_type=0            ! type of kernel to use in 'kernel' method (default=gaussian)
      stofields(jsto)%ker_coord=0           ! type of coordinates to use in 'kernel' method (default=grid)
      stofields(jsto)%ker_density=0._wp     ! density of kernels in 'kernel' method (default=0.0->not used)
```