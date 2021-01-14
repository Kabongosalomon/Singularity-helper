# How to generate a .sif from a Dockerfile
To produice a valid .sif you need to make the following .sif

- docker build -f Dockerfile . -t lab
- docker save -o lab.tar lab
- singularity build lab.sif docker-archive:///home/salomon/lab.tar
    - singularity build lab.sif docker-archive:///home/salomon/Singularity-helper/from\ Docker/lab.tar 

## Examples usage
- singularity exec lab.sif python

- srun -w devbox4 --gres=gpu:t2080ti:2 singularity exec -B /nfs/home/kabenamualus/:/run/user lab.sif jupyter-lab