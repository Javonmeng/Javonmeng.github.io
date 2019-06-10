---
title: final test
date: 2019-05-13 16:32:48
tags: 
    - 命令行
categories: 教程
---
#流程
```
git pull
hexo new post "article"

git add .
git commit -m "add a post"
git push

hexo d -g
hexo s
```
测试
```
startdocker -u "-it -e PYTHONPATH=/ghome/mengjw/perl5/LearningToPaint/lib" -c /bin/bash bit:5000/cxs-py36-tf112-torch041
```
PBS作业
```
#PBS -N LearningToPaint
#PBS -o /ghome/mengjw/perl5/LearningToPaint/LearningToPaint.out
#PBS -e /ghome/mengjw/perl5/LearningToPaint/LearningToPaint.err
#PBS -l nodes=1:gpus=1:S
#PBS -r y
cd $PBS_O_WORKDIR
echo Time is `date`
echo Directory is $PWD
echo This job runs on following nodes:
echo -n "Node:"
cat $PBS_NODEFILE
echo -n "Gpus:"
cat $PBS_GPUFILE
echo "CUDA_VISIBLE_DEVICES:"$CUDA_VISIBLE_DEVICES
startdocker -u "-e PYTHONPATH=/ghome/mengjw/perl5/LearningToPaint/lib" -c "python3 /ghome/mengjw/perl5/LearningToPaint/baseline/train_renderer.py" bit:5000/cxs-py36-tf112-torch041
```
提交和查看状态
```
qsub perl5/PBS_FILES/LearningToPaint.pbs 
sudo chk_res G105 mengjw
chk_gpuused G105
qstat -n|grep mengjw
```
