Instructions to run the gurobi model:

The notebook provided only contains the final gurobi model and the associated data files have been attached. Code used to obtain and curate the data from various sources is not presented here. Explanation of the notebook and steps to run:

Cell 1: Gurobi WLS License details
Cell 2: Loading data and libraries
Cell 3: Dataframe creation
Cell 4: Function to visualize output
Cell 5: Function to analyze and print output
Cell 6: Complete gurobi model
Cell 7: Model Outputs
Cell 8: Complete details for Hubs and Plants to be opened for the suggested solution

Considerations:
Parameters other than supply, demand, capacity can be edited in Cell 6 itself. All other parameters are manipulated using external code
Varying parameters has been observed to affect solution time massively, please consider adjusting MIPGap.
MIPGap should be adjusted. A 1-2% gap is recommended in most parametric cases for convergence. Smaller gaps may be extremely computationally intensive and MIP degeneracy may come into play for smaller gaps.

Steps to Run:
Run all cells sequentially.

Outputs:
In addition to outputs in the notebook, an image of the plot will be saved in the local directory of the model.
