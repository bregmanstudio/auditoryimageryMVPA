# auditoryimageryMVPA - fMRI decoding of auditory imagery
## Michael Casey, Lloyd May, Sean Paulsen, Dartmouth College

1. Setting up your environment

Make sure your environment has the following conda packages installed, in addition to standard Anaconda install (Python 2.x):
    pymvpa pybids

The examples below deploy the training and testing of classifiers using a TORQUE queue with "mksub".

cd ${YourExperimentDirectory}
git clone https://github.com/bregmanstudio/auditoryimageryMVPA.git
ln -s auditoryimageryMVPA/audimg.py .
ln -s auditoryimageryMVPA/qsub_audimg_subj_task.sh .
ln -s auditoryimageryMVPA/run_audimg_subj_task.qsub .

2. How to run classifier jobs on the queue
cd ${YourExperimentDirectory}
EDIT run_audimg_subj_task.qsub for your email address (if you want email notifications from discovery cluster queues)
EDIT audimg.py # set ROOTDIR to point to your working directory

Autoencoder classifiers
. qsub_audimg_subj_task.sh # This shell script will launch jobs to train/test classifiers for all subjects and all experiments (pch-class, pch-classX, timbre, timbre-X, pch-height)

Autoencoder results will be written to the following sub-directory in your current working directory:
results_audimg_subj_task_mkc_del0_dur1_SVDMAP_n10000_svd1.00_autoenc

Non-autoencoder classifiers
EDIT qsub_audimg_subj_task.sh # set autoenc=0
. qsub_audimg_subj_task.sh # This shell script will launch jobs to train/test classifiers for all subjects and all experiments (pch-class, pch-classX, timbre, timbre-X, pch-height)

Non-autoencoder results will be written to the following sub-directory in your current working directory:
results_audimg_subj_task_mkc_del0_dur1_SVDMAP_n10000_svd1.00

3. Seeing the results

cd ${YourExperimentDirectory}
ipython # launch an interactive python shell

In [1]: import audimg as A
In [2]: subj_res, grp_res = A.collate_model_results(tasks=['pch-class','pch-classX'], autoenc=1, n_null=10000, svdmap=1.0, show=True) 

This will output the following statistical summaries to the terminal, comparing autoencoder and non-autoencoder classifiers (or something like this, depending on your autoencoder params):

*******************************************************************
results_audimg_subj_task_SVDMAP_del0_dur1_n10000_autoenc_null
*******************************************************************
H
-----------------------------------------------------------------------
CLF: PCH-CLASS H
-----------------------------------------------------------------------
        ROI_key           ROI                    ACC        MIN/MAX    P (FDR)
        1034           lh-transversetemporal   0.1642  0.1250/0.1964   0.0039

I
-----------------------------------------------------------------------
CLF: PCH-CLASS I
-----------------------------------------------------------------------
        ROI_key           ROI                    ACC        MIN/MAX    P (FDR)
        1019           lh-parsorbitalis        0.1625  0.0893/0.2381   0.0368
        1024           lh-precentral           0.1607  0.1190/0.2262   0.0368
        1030           lh-superiortemporal     0.1684  0.1190/0.2202   0.0087
        1031           lh-supramarginal        0.1642  0.0952/0.2440   0.0355
        1035           lh-insula               0.1649  0.1310/0.2440   0.0163
        2001           rh-bankssts             0.1604  0.1131/0.2083   0.0180
        2020           rh-parstriangularis     0.1688  0.0952/0.2500   0.0368
        2024           rh-precentral           0.1719  0.1071/0.2321   0.0103
        2030           rh-superiortemporal     0.1726  0.1190/0.2560   0.0124
        2031           rh-supramarginal        0.1656  0.1250/0.2440   0.0251
        2035           rh-insula               0.1649  0.1190/0.2381   0.0124

I X
-----------------------------------------------------------------------
CLF: PCH-CLASSX I
-----------------------------------------------------------------------
        ROI_key           ROI                    ACC        MIN/MAX    P (FDR)
        2030          rh-superiortemporal      0.1628  0.1310/0.1964   0.0156

