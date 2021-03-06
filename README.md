BACKGROUND ESTIMATION
========

```
This script creates the Pdfs for the background contributions and extracts the alpha function for the W+jets background.

Setup Instructions
------------------

# Setup CMSSW in ~/private/FittingForATGC/Bacground/
cmsrel CMSSW_5_3_32
cd CMSSW_5_3_32/src
cmsenv

# Clone the repository
git clone git@github.com:muhammadansariqbal/FittingForATGCBackground.git

# Build the project
scram b -j 20

# Combine the originally created background trees
cd /afs/cern.ch/work/m/maiqbal/private/aTGC/
mkdir FittingInputTrees
cd FittingInputTrees
mkdir el
mkdir mu
cd ../Samples_80X_Working/
root -l ~/private/FittingForATGC/Background/CMSSW_5_3_32/src/FittingForATGCBackground/Modify_tree.cc

# Move the new trees
mv ../FittingInputTrees/ ~/private/FittingForATGC/Bacground/CMSSW_5_3_32/src/FittingForATGCBackground/InputTrees/

# Run the main script
python prepare_bkg_oneCat.py -b --channel el --readtrees
-b: batch mode
-c: channel (el or mu)
--readtrees: read the TTrees and save to RooDataHists (only needed once per channel)

# Plots are saved in plots_{channel}_HPV_900_3500
# The workspace is saved in cards_{channel}_HPV_900_3500
# The workspace has to be copied to the Input folder in the directory of the signal estimation script

---------------------

# New PDFs can be added in PDFs/HWWLVJRooPdfs.cxx, which has to be compiled in ROOT using (in this order)
# But before compiling the c scripts, we have to tell ROOT where Roofit headers are since CMS builds RooFit not as a part of ROOT.

gSystem->AddIncludePath("-I/cvmfs/cms.cern.ch/slc6_amd64_gcc472/lcg/roofit/5.32.03-cms/include/");

.L PdfDiagonalizer.cc+
.L Util.cxx+
.L hyperg_2F1.c+
.L HWWLVJRooPdfs.cxx+

