aPRBind is a method for the prediction of RNA-binding residues on proteins.
Authors are Yang Liu, Weikang Gong, Yanpeng Zhao, Xueqing Deng, Shan Zhang and Chunhua Li.
The performance process includes three steps: I-TASSER model construction, feature extraction and prediction performance.

Taking the binding site prediction of an RNA-binding protein for example, whose sequence file is 2l5d_a.FASTA (Here, a is the chain identifer. For the protein, the experimental complex structure has been solved with PDB code 2l5d), the following shows the process of the prediction.


The first step: I-TASSER model construction
Go to the website https://zhanglab.ccmb.med.umich.edu/I-TASSER/ to construct the protein structure, and the first model (named as 2l5d_a.pdb) out of the five predicted ones is selected. Parameters are set to default values.   


The second step: Features extraction

1. AA index：
AA index is from the website: https://www.genome.jp/aaindex/ 
AAIndex ID	Description
FINA910101	Helix initiation parameter at position i-1
OOBM850101	Optimized beta-structure-coil equilibrium constant
TANS770108	Normalized frequency of zeta R
TANS770106	Normalized frequency of chain reversal D
WOEC730101	Polar requirement
LEWP710101	Frequency of occurrence in beta-bends
ISOY800105	Normalized relative frequency of bend S
FAUJ880108	Localized electrical effect
RICJ880105	Relative preference value at N2
COSI940101	Electron-ion interaction potential values
Charge type 
Run AA_index.py (command line: python ./AA_index.py) with 2l5d_a.pdb as input file (keep AA_index.py and 2l5d_a.pdb in the same folder), and then get 2l5d_a_AA_index.xlsx

2. SPIDER3-based features
Get SPIDER3-based features from the website: https://sparks-lab.org/server/spider3/ 
Input query sequence, and then name the output file as 2l5d_a.spd33. 

3. SNB-PSSM
Go to the website https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastp&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome to get a pssm file named 2l5d_a.asn_matrix.txt 
Calculate SNB-PSSM based feature with all the associated Matlab m files including clash.m and snbpssm.m kept in the same folder. And 2l5d_a.pdb and 2l5d_a.asn_matrix.txt are in the folder where Matlab m files are.
In Matlab, run the command line: clash('2l5d_a.pdb','A','2l5d_a.asn_matrix.txt'), and get 2l5d_a.xlsx. Then, run the command line: snbpssm('2l5d_a.pdb','A','2l5d_a.xlsx','2l5d_a'), and get 2l5d_a_SNB-PSSM.xlsx

4. Topology
Use NetworkX program pocket to calculate the topology features with Cα atoms as nodes and distance cutoff = 7.5 Å
keep the program GNtopology.py and 2l5d_a.pdb files in the same folder, and the current path is set to the path of the folder. This setting is in line 104: filePath=‘the folder path’
Run GNtopology.py file (command line: python ./GNtopology.py), and get a 2l5d_a_Network_statistics.txt file.

5. IP
The residue-nucleotide propensities (60×8) are in the file "60x8.txt".
Run Ip.py (command line: python IP.py spd33.xls 60.xls) with 60.xls and spd33.xls (copy the content of 2l5d_a.spd33 into a xls format file) as input files, and then get 2l5d_a_IP.txt
Here keep IP.py, spd33.xls and 60.xls in the same folder.  

6. CX/DPX
Go to the Website：https://sourceforge.net/projects/psaia/ to download and install the program psaia.exe.
a. Run “psaia.exe”;
b. Step by step, selecet "Structure Analyser", then under it select "Analysis Type" and then select "Analyse by Chain". All parameters are set to default.  
Input the pdb file 2l5d_a.pdb (hydrogen atoms removed) to the program, and click “run” to get the result, i.e.,2l5d_a.tbl file.

7. Dynamics
Calculate the dynamics features with all the associated Matlab m files including GNM.m, pdbread.m and juli.m kept in the same folder,
In Matlab, command line is GNM('2l5d_a.pdb') and here 2l5d_a.pdb is in the folder where Matlab m files are.
Get a 2l5d_a_dynamics.txt file with dynamic features.

After all the features are extracted, all the featues files obtained above including 2l5d_a_SNB-PSSM.xlsx, 2l5d_a_AA_index.xlsx, 2l5d_a.spd33, 2l5d_a.tbl,2l5d_a_IP.txt, 2l5d_a_Network_statistics.txt, and 2l5d_a_dynamics.txt,need to be put into a single file 2l5d_a_feature.xlsx.
Run Feature_integration.py (command line: python ./Feature_integration.py) with all the features files kept in the same folder where Feature_integration.py is located. 
To run Feature_integration.py, the python modules xlrd, numpy and xlsxwriter need to be installed in python.
 

The third step: prediction performance
command line: python ./CNNtest.py  
keep CNNtest.py,model folder, and 2l5d_a_feature.xlsx in the same folder. 
The output is 2l5d_a_result.txt where the prdicted RNA-binding sites are stored.

