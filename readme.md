# Finite-Capacitance

### Background:
Capacitance of parallel plate capacitors is easily calculable for the domains where the plate seperation is much smaller than the area of the plates, `s << sqrt(A)`. For domains where `s` is comparable to `sqrt(A)` edge effects and fringe feilds affect the capacitance in a non negligible way. When the plate area is much larger than seperation distance, the electric field between the two plates is highly uniform resulting in a capacitance very close to the ideal case where edge effects do not exist. In this case, the charge distribution along the plates is very uniform.

For the domain where `s` is comparable to `sqrt(A)`, the charge distribution is much less uniform due - as is the electric field between the plates. Because of the requirement that both plates are at the same potential, the charge distributions on both plates must move around into a configuration where the resultant electric fields produce the same potential difference between both plates along all paths that the electrons will move through to get from plate to plate. 

### This Project:
This project consists of two parts: 
1) To numerically calculate the potential and thus capacitance across the two plates with a uniform charge distribution. 
    - We can test this by comparing the ideal case domain and the non ideal case domain:
        - In the ideal domain, where `s << sqrt(A)`, we should find that the potential difference across the two plates has a very low variance. In other words, in this configuration assuming a uniform charge distribution should not break our boundary condition of the top plate being at potential V+ and the bottom at V-, resulting in a difference of V between the two plates along all paths through the electric field between the two plates.
        - In the non ideal domain, we should find that the assumption of uniform charge distribution will violate our imposed boundary conditions and result in potential differences which change depending on which part of the plate the charge leaves from and arrives at.
2) To numerically determine the charge distribution required to satisfy the boundary conditions imposed above. 
    - This is fundementally a constraint satisfaction problem. 
    - Many current techniques involve using FDM and FEM methods to satisfy Laplace's equation.
    - It may be possible to solve this as an optimization problem utilizing local search methods such as stochastic gradient descent, stocastic beam search, and other numerical solutions to optimization problems.

### Plan:
1) Calculate capacitance along one path between plates
    - e.g., capacitance at the edge of a capacitor may be different from capacitance at the center of the capacitor
    - i.e., when making an assumption of the charge density, we may be inaccurate resulting in a varying potential difference along a path between the two plates, resulting in a different capacitance.
    - Test: ensure that capacitance at center of plate is (e_0)*A/d
2) Average the capacitance along all paths between plates to determine total capacitance 
    - calculate capacitance everywhere and then average it
    - Test: ensure that capacitance for case d << sqrt(A) is (e_0)*A/d
3) Attempt to determine charge density by solving C.S.P
    - V(top) = V+
    - V(bottom) = V-
    
### Implementation:
1) Calculate Capacitance along one path
    1) Define Domain
        - Lx, Ly, Lz = length in x, y, & z in meters
        - Nx, Ny, Nz = # discrete elements in x, y, &z
        - Dx, Dy, Dz = Lx/Nx, Ly/Ny, Lz/Nz = length / element in direction 
        - Domain Tensor = tensor of latice elements Nx by Ny by Nz in size
    2) Define Conductor Domain
        1) Generate a "plate"
            - rectangular = f(lx, ly, lz) where lx,ly,lz are the dimensions of the plate 
            - circular = f(r) where r is radius
        2) Mirror plate, seperate, and center
            - Determine X,Y position of plate elements to center it in x,y plane
            - Determine Z position of plates to center both in z dimension
            - generate a 2d-tensor n by 3, where n is amount of elements to contain conductor and the 2nd dimension contains the x,y,z coordinates
    3) Integrate by summation
        - ![V = -\int_{+}^{-} \overrightarrow{E}*d\overrightarrow{l} = \int d\overrightarrow{l} * \int \rho(\overrightarrow{r}') * \frac{(\overrightarrow{r} - \overrightarrow{r}')}{|(\overrightarrow{r} - \overrightarrow{r}')|^3} * d\tau'  \propto Q](https://cloud.githubusercontent.com/assets/10381896/25315834/c2687750-2829-11e7-9740-15bf097ded52.gif)