# gromacsworkflow
submit gromacs job on sge system

Before use submit.sge, make sure:
0\could assign a host by changing @"hostname" in line below or just leave it blank
#$ -q yourhostname.q@g0353

1\put your "*.pdb", all "*.mdp" and "submit.sge" files in a new folder

2\change your job name in these lines:
"#$ -N jobname.pdb" 

and

"JOBNAME=jobname.pdb"

3\
a. check your pdbfile and change chain A to chain B. REMENBER to check the line number and modify "71" below.
sed -i '1,69s/ A   / B   /' $JOBNAME
b. REMEMBER to change force field below.
echo 15 | gmx pdb2gmx -f $JOBNAME -o _processed.gro -water spce

4\submit your job using:
qsub submit.sge

5\check the job state, showing "r" means it works well. using:
qstat

6\grep the ID of your job, to check the step using:
cat *.pdb.o"jobID"

7\check the resource and CPU using:
qhost

ssh "your host"

top

q

8\Wait and Goodluck :)

9\run:
gmx make_ndx -f md_0_1.gro
gmx hbond -s md_0_1.tpr -f md_0_1.xtc -n index.ndx -num hbnum.xvg
echo 0 0 | gmx trjconv -s md_0_1.tpr -f md_0_1.xtc -o md_0_1_noPBC.xtc -pbc mol -center
echo 4 4 | gmx rms -s md_0_1.tpr -f md_0_1_noPBC.xtc -o rmsd.xvg -tu ns


10\check the rmsd.xvg, choose suitable time scale and vim pbsasubmit.sge to write -b ** -e ** time; check if echo is right or not.

11\qsub pbsasubmit.sge
