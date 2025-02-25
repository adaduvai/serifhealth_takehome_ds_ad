# Overview of approach
## Business approach
To solve the problem, I found the payment rates for a given procedure at a given provider reported by both the payer and the provider, and then chose the one I found to be most appropriate.

## Technical approach
1. Read in the payer and hospital data, and perform data type conversions for columns that don't read in correctly
2. For both of the data sets, create a distribution report of the data as-is
3. Review and summarize the data sets, noting down any important fields for the business problem as well as fields with the same semantic meaning either within or across the two data sets
4. Adjust the columns that have the same semantic meaning across teh two data sets so that the values match exactly between them (i.e., so that they can be used as merge keys)
5. Merge the payer and provider reimbursement data sets across all merge keys available (payer, provider, procedure code, procedure code type, charge methodology / negotiation type) to create a single data set with prices from both payer and provider for a given procedure
6. Review the distributions of all reimbursement rates across plan and procedure code
7. To simplify the approach, pick the best rate from reviewing the distribution of reimbursement rates for a given plan and procedure code

## Assumptions

## Future work
It would be challenging to scale this approach to more payers/plans, providers, and procedure codes. Only 2 plans, 1 hospital, and 2 procedure codes are used here, and so it's reasonable in this case to use visual inspection of the distribution to select the best rate, but visual inspection would not work very well across more plans, more hospitals, and more procedure codes.


# Instructions on how to run code
Assumptions: Anaconda Python is installed.
1. Install the pandas-profiling library to your Anaconda environment (`pip install pandas-profiling`)
2. Clone the repository to your local machine
3. Place the data files `tic_extract_20250213.csv` and `hpt_extract_20250213.csv` within the folder `serifhealth_takehome_ds_ad`
4. Open a Jupyter Notebook server
5. Open the notebook `Serif Health Take-Home Assessment.ipynb`
6. In the top bar of the Juypyter Notebook, click Cell > Rull All

# Any other relevant information that would help in understanding and evaluating your work
