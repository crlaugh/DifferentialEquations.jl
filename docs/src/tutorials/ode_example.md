# Ordinary Differential Equation (ODE) Example

This tutorial will introduce you to the functionality for solving ODEs. Other
introductions can be found by [checking out the IJulia notebooks in the examples
folder](https://github.com/ChrisRackauckas/DifferentialEquations.jl/tree/master/examples).

In this example we will solve the equation

```math
du/dt = f(u,t)
```

where ``f(u,t)=αu``. We know via Calculus that the solution to this equation is
``u(t)=u₀*exp(α*t)``. To solve this numerically, we define a problem type by
giving it the equation and the initial condition:

```julia
α=1
u₀=1/2
f(u,t) = u
prob = ODEProblem(f,u₀)
```

Then we setup some parameters:

```julia
Δt = 1//2^(4) #The initial step size. It will automatically determined if not given.
tspan = [0,1] # The timespan. This is the default if not given.
```

We then send these items to the solver.

```julia
sol =solve(prob::ODEProblem,tspan,Δt=Δt,save_timeseries=true,alg=:Euler)
```

Plotting commands are provided via a recipe to Plots.jl. To plot the solution
object, simply call plot:

```julia
plot(sol)
#Use Plots.jl's gui() command to display the plot.
Plots.gui()
#Shown is both the true solution and the approximated solution.
```

### Other Algorithms

We can choose a better algorithm by specifying:

```julia
sol =solve(prob::ODEProblem,tspan,Δt=Δt,save_timeseries=true,alg=:ExplicitRK)
plot(sol)
Plots.gui()
```

The `"ExplicitRK"` algorithms are general Runge-Kutta solvers. It defaults to
Dormand-Prince 4/5, the same solver as MATLAB's `ode45`. Please see the solver
documentation for more algorithms.

We can solve the problem in less timesteps by turning on adaptive timestepping. To
do so, you simply pass a keyword argument:

```julia
sol =solve(prob::ODEProblem,tspan,Δt=Δt,save_timeseries=true,alg=:ExplicitRK,adaptive=true)
plot(sol)
Plots.gui()
```

### Systems of Equations

We can also solve systems of equations. DifferentialEquations.jl can handle any
size problem, so instead of showing it for a vector, let's let u be a matrix!
To do this, we simply need to have u₀ be a matrix, and define f such that it
takes in a matrix and outputs a matrix. We can define a matrix of linear ODEs
as follows:

```julia
u₀=rand(4,2).*ones(4,2)/2
α=ones(4,2)
f(u,t) = α.*u
prob = ODEProblem(f,u₀)
```

Here our ODE is on a 4x2 matrix. Since we are using .\*, this is 8 independent
ODEs, but you can do whatever you want. To solve the ODE, we do the same steps
as before.

```julia
sol =solve(prob::ODEProblem,tspan,Δt=Δt,save_timeseries=true,alg=:ExplicitRK)
plot(sol)
Plots.gui()
```