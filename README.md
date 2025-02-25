# Overview of approach
## Summary of business approach
To solve the problem, I found the payment rates for a given procedure at a given provider reported by both the payer and the provider, and then chose the one I found to be most appropriate bsaed on my understanding of payer-provider reimbursements. From reviewing the data, I picked the median of payer reimbursement rate as the best single reimbursement rate to use.

I used the median to aggregate across the multiple values that I had for each code and plan type. This is because there are some high outliers that skew the mean upwards.

I used payer reimbursement rate over the other reimbursement rates for the following reasons:
* CMS reimbursement rate: CMS reimbursement rates tends to apply only for Medicare/Medicaid plans, and also tend to be lower than those that private/commericial insurance have.
  * My hypothesis is that these are present in the payer's data to act as a lower benchmark for what the payer might be willing to pay to the provider.
* Provider reimbursement rates (standard charge - gross, standard charge - max, standard charge min): Per my understanding, the provider reimbursement rate is likely only a starting point in negotiations with the payer, while the payer reimbursement rate likely reflects the actual amount that the payer pays to the provider.
* Provider rate of standard charge - discounted cash: This might only be applicable to a specific subset of scenarios where the patient pays cash to the hospital instead of the patient involving their insurance.

## Summary of technical approach
1. Read in the payer and hospital data, and perform data type conversions for columns that don't read in correctly
2. For both of the data sets, create a distribution report of the data as-is
3. Review and summarize the data sets, noting down any important fields for the business problem as well as fields with the same semantic meaning either within or across the two data sets
4. Adjust the columns that have the same semantic meaning across teh two data sets so that the values match exactly between them (i.e., so that they can be used as merge keys)
5. Merge the payer and provider reimbursement data sets across all merge keys available (payer, provider, procedure code, procedure code type, charge methodology / negotiation type) to create a single data set with prices from both payer and provider for a given procedure
6. Review the distributions of all reimbursement rates across plan and procedure code
7. To simplify the approach, pick the best rate from reviewing the distribution of reimbursement rates for a given plan and procedure code

## Assumptions
* The date that the reimbursement value was last updated (network year-month in the payer data, last updated on in the hospital data) is not meaningful to calculating reimbursement rate.
* Billing class is not meaningful to calculating reimbursement rate.
* In the charge methodology field of the hospital data, "Case Rate" means the same thing as "negotiated" in the negotiation type field.
* The payer name of "UHC" in the hospital data means the same thing as the payer "unitedhealthcare" in the payer data.

## Future work
It would be challenging to scale this approach to more payers/plans, providers, and procedure codes. Only 2 plans, 1 hospital, and 2 procedure codes are used here, and so it's reasonable in this case to use visual inspection of the distribution of rates to select a single best rate, but visual inspection would not work very well across more plans, more hospitals, and more procedure codes, as this would take too much time.

In addition, in the final payer/hospital joined data set, the values of payer are aligned with those of negotiation type, and the values of setting and description are aligned with those of plan name; however, this would almost certainly not be the case with real world data.

Instead, a more robust approach combining descriptive statistics with business knowledge would work better. Assuming sufficient business knowledge of the available data, one could, e.g., compute a weighted average of the relevant reimbursement rates provided to calculate a final reimbursement rate. These could even be weighted differently based on the dimensions noted above -- payer/plan, provider, procedure code, setting, and negotiation type. For example, if a plan were a Medicare plan, it would likely be appropriate to weight the CMS reimbursement rate from the payer data more highly. It might also be desirable to do additional data cleaning before calculating these values.

# Instructions on how to run code
Assumptions: Anaconda Python is installed.
1. Install the pandas-profiling library to your Anaconda environment (`pip install pandas-profiling`)
2. Clone the repository to your local machine
3. Place the data files `tic_extract_20250213.csv` and `hpt_extract_20250213.csv` within the folder `serifhealth_takehome_ds_ad`
4. Open a Jupyter Notebook server
5. Open the notebook `Serif Health Take-Home Assessment.ipynb`
6. In the top bar of the Juypyter Notebook, click Cell > Rull All

# Any other relevant information that would help in understanding and evaluating your work
