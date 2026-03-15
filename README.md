### Simulation of elastic particle networks with binding dynamics 

I have created a Brownian dynamics simulation of the overdamped Langevin equations corresponding to a particle network with elastic connections and firm binding dynamics. 

![Simulation example](media/demonstration.gif)

*Example simulation: 5-particle fully connected network. Green: unbound, red: bound. The dotted lines indicate the elastic connections between partcles.*

#### Physics: 
System design inspired by complex diffusion behaviour exhibited by real world biophysical systems, such as the assembly of SAS-9 rings. The system dynamics consist of: 
- Brownian motion of the individual particles
- Harmonic spring forces
- Stochastic firm bond dynamics with rates 'q_on' and 'q_off'

#### Features
All relevant parameters of the simulation are adjustable, it works in one and two dimensions. Any possible particle network can be handled by the simulation, additionally there are functions to set up the initial conditions for the most relevant ones. The data from the simulations can be saved in json format with the function 'write_hist', the used parameters are saved as well. The function 'plot_hist' creates an interactive plot showing the systems evolution (as seen in the featured gif) 

There is a second program titled 'analysis' which shows some evaluations of the data I performed in my bachelor thesis. There is a useful function 'imports_hists' to import the stored json-files into workable numpy arrays, and further useful functions to enable easier handling of the data. In this part of the analysis, the diffusion constants of the runs was computed with the help of the function 'get_diffusion' and compared to theoretical results I obtained analytically in my thesis. 

#### Requirements 
Jupiter Notebook with Python 3.11.14 kernel. 
Needed packages: 
- NumPy
- Matplotlib
- time
- json
- warnings
- glob
- tqdm
- scipy
- os
- support for %matplotlib widget to use interactive plotting


#### Usage 

Example usage for a simple system of two particles in one dimension. 
```python
#example function call
num_elems = 2                                                        #simulation with two elements
pos_init = np.array([[-1/2], [1/2]], dtype = "float")                #sets initial positions
conn_matrix = connectivity_setup_chain(num_elems)                    #sets connections between elements
l0 = natural_rest_lengths(pos_init)                                  #computes the natural rest length based on init_pos
q_on = 0                                                             #no binding dynamics
q_off = 0
for i in tqdm(range(1)):                                             #useful for running multiple simulations, here only 1
    bonds_init = bonds_init_setup(num_elems,q_on,q_off)              #get appropriately random initial conditions for bonds                                                                       #set up class System to run the simulation
    sys = System(num_elems, pos_init, bonds_init, conn_matrix, q_on = q_on, q_off = q_off, l0 = l0, dt = 1e-4)   
    sys.run(1e3,1e2)                                                 #run the system for 1e3 steps, sample every 1e2 steps
    sys.calculate_diffusion_const()                                  #calculate diffusion constant
    sys.plot_hist()                                                  #create interactive plot
    sys.write_hist("2part_1d/no_binding")                            #write run data to json
```
