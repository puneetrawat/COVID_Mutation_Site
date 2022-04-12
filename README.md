# COVID_Mutation_Site
All the supplementary data including datasets/ML-models 
<hr>
<h4>Background:</h4>

The prolonged transmission of the severe acute respiratory syndrome coronavirus 2 (SARS-CoV-2) virus in the human population has led to demographic divergence and the emergence of several location-specific clusters of viral strains. Although the effect of mutation(s) on severity and survival of the virus is still unclear, it is evident that certain sites in the viral proteome are more/less prone to mutations. In fact, millions of SARS-CoV-2 sequences collected all over the world have provided us a unique opportunity to understand viral protein mutations and develop novel computational approaches to predict mutational patterns. In this study, we have classified the mutation sites into low and high mutability classes based on viral isolates count containing mutations. The physicochemical features and structural analysis of the SARS-CoV-2 proteins showed that features including residue type, surface accessibility, residue bulkiness, stability and sequence conservation at the mutation site were able to classify the low and high mutability sites. We further developed machine learning models using above-mentioned features, to predict low and high mutability sites at different selection thresholds (ranging 5-30% of topmost and bottommost mutated sites) and observed the improvement in performance as the selection threshold is reduced (prediction accuracy ranging from 65-77%). The analysis will be useful for early detection of variants of concern for the SARS-CoV-2, which can also be applied to other existing and emerging viruses for another pandemic prevention.

<h4>Content:</h4>

This repository contains the datasets (generated using Weka 3.8.6) used in the study. However, it is important to note that it does not contains code used to prepare the dataset due to use of third party softwares and logistic problems. 

This readme file contains detailed information on how to calculate the features and develop ML model using Weka 3.8.6.

Feel free to contact us if you have any further quries (puneet.rawat@medisin.uio.no)

<h5>Protein structures:</h5>
The structures of SARS-CoV-2 proteins were obtained from https://zhanggroup.org//COVID-19/. 

<h5>Calculation of features:</h5>

There were total of four features used in the ML-model:

**(1) Residue type (Res) at the mutation site:** The amino acid at the mutation site are divided into five catagories: 

1. Non-Polar = ["G","A","V","L","M","I"]
2. Polar = ["S","T","C","P","N","Q","H"]
3. Aromatic = ["F","Y","W"]
4. Positive charge = ["R","K"]
5. Negative charge = ["D","E"]

The features were converted to numerical values to test various ML models.

The class of residue type was changed according to their numbering (as shown above). 
For example, if the residue at the mutation site is a non-polar residue "G" then its feature value is "1", feature value for polar residue "S" is "2", "3" for aromatic residue "F" and so on...

 

**(2) Residues flanking the mutation site (Resflank):** reisdues present at the N-terminal and C-terminal side of the mutation site were extracted. These dipeptide were also converted to numerical values based on the below criteria.

A	1;
C	2;
D	3;
E	4;
F	5;
G	6;
H	7;
I	8;
K	9;
L	10;
M	11;
N	12;
P	13;
Q	14;
R	15;
S	16;
T	17;
V	18;
W	19;
Y	20

Example: Mutation site has residue "M (MET)". It has residue "N (GLU)" on the N-terminal side and "C (CYS)" on C-terminal side. Therefore, mutation site in protein looks like "NMC"
The position on N or C terminal does not matter in the feature. Therefore, we used smaller digit in the first position
So the feature value of NC==CN==212

Note: Using smaller digit on first position also avoids confusion for some position such as AV 1,18 can also mean MI 11,8. But fixing first position for smaller digit leads to (AV or VA: 118) and (MI or IM: 811).

**(3) Relative accessible surface area (rASA) of the mutation site:** the residue-wise rASA was calculated for each protein using Dictionary of Secondary Structure of Proteins (DSSP). A detailed description of the program is available at https://swift.cmbi.umcn.nl/gv/dssp/DSSP_3.html

**(4) Average value of position weight matrix (PWMavg):** We have generated PSSM file for each protein using internally seting up the "UniRef90" database (https://www.ebi.ac.uk/training/online/courses/uniprot-quick-tour/the-uniprot-databases/uniref/) and using Blastpgp command (http://nebc.nerc.ac.uk/bioinformatics/documentation/blast/blastpgp.html).

Note: "UniRef90 is built by clustering UniRef100 sequences such that each cluster is composed of sequences that have at least 90% sequence identity to, and 80% overlap with, the longest sequence in the cluster (the seed sequence)."

The SARS-CoV-2 protein positions containing mutation(s) and above calculated features are added in the GitHub repository.

<h4> Model development: </h4>

We have used open-source machine learning development software "Weka (version 3.8.6)" for the study. Here we have explained using the dataset(s) to check the prediction performance of the model.

1. In the "Preprocess" tab, click on the "Open file"
2. Select any of the dataset (30% selection cutoff is baseline model)
3. Now you can four features (racc_dssp, pssm_score,wild-type,dipeptide_palendrome) representing above explained features. The last feature (class_full) is binary classification of the mutation site (high mutability class or low mutability class)
4. Now go to "Classify" tab and choose the "Classifier".
5. Select "Weka -> classifiers -> functions -> SMO". You can now see the default parameters for the support vector machine model.
6. Click on the parameter tab next to "choose" classifier option. It will open a new GUI window.
7. Click on "buildCalibrationModels" drop down option and select "True". change the value of "c" to 2.0.
8. Click "OK" and select "Use training dataset" radio button option in the "Test options" section.
9. you can how see the performance of the model at "Classifier output" section.

See weka documentation here (https://www.cs.waikato.ac.nz/ml/weka/) for more details.
