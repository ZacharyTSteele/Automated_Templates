**Background**

Stable isotope analysis (SIA) is a powerful tool that has been a staple of the fields of oceanography, geochemistry, and forensic sciences. Recently, the application of SIA has expanded to other fields such as ecology and animal biology. However, the incorporation of SIA to ecology and animal biology is often limited to carbon and nitrogen isotopes. In addition, many researchers in the fields of ecology and animal biology choose to send their samples to other institutions for analysis instead of completing their analyses in-house because of the expenses and logistics related to traditional isotope analysis. The development of new instrumentation such as cavity ring-down spectroscopy (CRDS) and off-axis integrated cavity output spectroscopy provide a potential solution to these setbacks. In particular, CRDS is an affordable alternative to the long-standing traditional reliance on isotope ratio mass spectrometry (IRMS). CRDS instruments, such as the Picarro L2140-i, provide high-precision measurements of hydrogen and oxygen stable isotopes in a fraction of the time required for IRMS analysis, and CRDS instruments possess automated functionality, allowing for a less labor-intensive process compared to IRMS.
	

Stable isotopes of oxygen and hydrogen (measured as δ17O, δ18O, and δ2H) can provide information related to the sources of environmental water intake and to the animal’s body water pool. Most hydrogen and oxygen analyses use distilled blood plasma or serum samples, but under the right circumstances (e.g., a trained animal in captivity) distilled saliva and urine samples can be collected as well. A central premise for all these methods is that water is critical to animal biology. Most terrestrial animals are ~60-70% water by mass, and this body water comes from a combination of environmental sources and endogenous processes (e.g., newly-synthesized metabolic water, a byproduct of metabolic pathways.
	The code stored in this repository (available for both R and Python) is designed to help expand the use of stable isotope analysis via CRDS to the fields of ecology and animal biology. While CRDS instruments such as the Picarro L2140-i are user-friendly water isotope analyzers, the concepts related to SIA are still daunting and may be intimidating for many ecologists or animal biologists who may consider incorporating this type of instrumentation into their lab. Many researchers continue to rely on shipping their water or plasma samples to outside institutions for analysis due to this obstacle. The purpose of this repository is to provide simple and efficient code to demonstrate the ease with which a researcher can correct and finalize data from a CRDS water analyzer instrument for publication. For example, while the raw datasheet exported from a CRDS instrument like the Picarro L2140-i provides data related to >25 variables, the variables of interest are as follows: 
1)	δ17O (in the code as ‘d(17_16)Mean’ for raw values and ‘d17O_amended’ for corrected) 
2)	δ18O (in the code as ‘d(18_16)Mean’ for raw values and ‘d18O_amended’ for corrected)
3)	Δ17O (calculated from δ17O and δ18O; in the code as ‘E17_Mean’ for raw values and ‘E17O_amended’ for corrected)
4)	δ2H [deuterium;2H] (in the code as ‘d(D_H)Mean’ for raw values and 'dHamended' for corrected which is then used with the d18O_amended value to calculate 'D-excess' [deuterium-excess]) 
5)	average water concentration (in the code as ‘H2O_Mean)

Ultimately, the code provided in this repository generates an exportable datasheet with just these variables of interest.
This code is designed to accomodate the differences in standards and control waters used for each lab. As such, an important component of this code is loading the established or previously measued values of all the waters typically analyzed during run (these values are typically set during a standards run as described in the manuscript). In addition, while running the Picarro, it is important to consistently name your waters and label them as either standard, conditioning, control, or unknown (using both identifier 1 and identifier 2 within the 'samples description' of the CRDS coordinator). This code is currently built to accommodate a 2-point (two standards) correction but can easily be adjusted for a 3-point linear correction. If everything is named and labeled correctly, the code then builds a correction equation by comparing the known and measured values of the two standards for δ17O and δ18O. The individual correction equations are then applied to the raw δ17O and δ18O values of each water included in the run, allowing for generation of corrected values for δ17O, δ18O, and Δ17O. The corrected values are then compared to known and established in-house values to ensure that the run was successful. 

The code provided should effectively cycle through a run nearly autonomously (any section requiring manual changes is bolded in the R and Python script) and provide easy to interpret data output via a an xlsx file that includes a comparison section that allows for easy determination if described benchmarks below are met. 
	To provide support for this, I have provided an example raw datasheet (Test_Data.csv). Running this file in either the R or Python code should demonstrate the following: 
1)	That generating a correction equation from VSMOW and SLAP measurements during standards runs should generate: 
a.	δ17O values within 0.15‰ (per mil) of the known/established value
b.	δ18O values within 0.3‰ of the known/established value
c.	Δ17O values within 0.015‰ of the known/established value

**FILES INCLUDED **

Automated_Post_Processing_R_Code - Post-processing R script 

Automated_Post_Processing_Python_Code - Post-processing Python script

Test_Data.csv - Example raw datasheet from the Picarro which can be used with either the R or Python scripts to test the provided code

Values.RData - RData file that loads in the known and established values for water standards and samples routinely measured in the lab (for R)

Values2.py - Py file that loads in the known and established values for water standards and samples routinely measured in the lab (for Python)

**OUTPUTS**

csv.files – Each code generates csv files for each individual water sample analyzed during the run. These csv files contain all the variables measured by the CRDS instrument and are mainly generated as a backup source if further analysis was ever required. These files are generated in the following format: SampleName_Day_Year_Month.csv 

.xlsx files – At the conclusion of each code, a single xlsx file will be generated that contains all the finalized data for each water sample as a sheet within the xlsx file. These sheets only contain the variables of interest and are meant to be used for publication and for generating larger compiled datasheets. These files are generated in the following format: Unknonwns_Day_Year_Month.xlsx