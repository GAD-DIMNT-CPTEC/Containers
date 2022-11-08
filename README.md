# Containers

Repositório para alocar arquivos de definição de containers (Singularity, Docker etc).

* Arquivo `BAM_V1.2.1.def`: arquivo de definição do singularity para compilar o modelo BAM_V1.2.1 utilizando o GCC 4.8.5 e MPICH 3.3 (utiliza o Ubuntu 18.04 como base);
* Arquivo `BAM_V1.2.0.def`: arquivo de definição do singularity para compilar o modelo BAM_V1.2.0 utilizando o GCC 4.8.5 e MPICH 3.3 (utiliza o Ubuntu 18.04 como base);
* Arquivo `SCANTEC_V2.0.0.def`: arquivo de definição do singularity para compilar o SCANTEC V2.0.0 utilizando o GCC 9.0 (utiliza o Ubuntu 20.04 como base);
* Arquivo `ubuntu_Eval_V1.0.0.def`: arquivo de definição do singularity para compilar o readDiag utilizando o GCC 4.8.5 (utiliza o Ubuntu 18.04 como base), incluindo o Miniconda e um ambiente chamado readDiag;
* Arquivo `ubuntu-18.04_gcc-4.8.5_mpich-3.3.def`: arquivo de definição do singularity para compilação do oensMB09 e BAM_V1.2.1 utilizando o GCC 4.8.5 e o MPICH 3.3 na Egeon (utiliza o Ubuntu 18.04 como base).
* Arquivo `xc50_dev.def`: arquivo de definição do singularity para compilação do oensMB09 e BAM_V1.2.X utilizando o GCC 5.5.0, MPICH 3.1.3 e LAPACK/BLAS 3.4.2 (instalados a partir do PETSC 3.7.6 - da mesma forma como na máquina Cray XC50), utiliza o Ubuntu 18.04 como base.

## Uso

Na máquina Egeon, utilize o `singularity` para construir o container. Por exemplo:

```
module load singularity
singularity build --fakeroot arquivo.sif arquivo.def
```

No final do processo, será gerado o arquivo `arquivo.sif`.

## Aplicação

Dependendo o container, a aplicação do container pode ser feita de duas forma (estou atribuindo nomes que talvez não sejam os mais corretos ou representativos do modo de uso):

### Isolado

Neste tipo de aplicação, o container pode ser utilizado de forma isolada em qualquer máquina que tenha o singularity instalado e recursos suficientes. Um exemplo deste tipo de aplicação são os containers `BAM_V1.2.x.def`, `SCANTEC_V2.0.0.def` e `ubuntu_Eval_V1.0.0.def` que trazem as aplicações instaladas dentro do container. Nesse caso, o uso depende do tipo de aplicação. De qualquer maneira, pode-se abrir um shell a partir do container e fazer uso da aplicação.

#### Exemplo

Para o container `BAM_V1.2.x.def`, os executáveis do pré-processamento, modelo e pós-processamento encontram-se no diretório `/usr/local/bin` dentro do container. Na máquina Egeon, para abrir um shell a partir do container, utilize o comando a seguir:

```
$ module load singularity
$ singularity shell -e --bind /mnt/beegfs/$user:/mnt/beegfs/$user BAM_V1.2.1.def
```

Com isso, será aberto um prompt que permite a utilização das ferramentas e aplicações que o container possui. O namespace, arquivos e permissões do usuário que estiverem no diretório `/mnt/beegfs/$user` serão expostos para o container no mesmo diretório de bind.

### Embutido 

Nesse tipo de aplicação, o container é utilizado como um ambiente para a compilação e execução da aplicação utilizando, por exemplo, o agendador da máquina host (e.g., PBS ou SLURM). Na máquina Egeon, para utilizar as ferramentas GCC 4.8.5 e MPICH 3.3 a partir do container `ubuntu-18.04_gcc-4.8.5_mpich-3.3.def` em um script de submissão que utiliza o MPI, realize a seguinte substituição (apenas um exemplo):

```
aprun -n ${MPPWIDTH} -N ${MPPNPPN} -d ${MPPDEPTH} -ss \${EXECFILEPATH}/ParModel_MPI < \${EXECFILEPATH}/MODELIN > \${EXECFILEPATH}/setout/Print.model.${LABELI}.MPI${MPPWIDTH}.log
```

para

```
singularity exec -e --bind /mnt/beegfs/carlos.bastarz:/mnt/beegfs/carlos.bastarz /mnt/beegfs/carlos.bastarz/containers/egeon_dev.sif mpirun -np ${MPPWIDTH} \${EXECFILEPATH}/ParModel_MPI < \${EXECFILEPATH}/MODELIN > \${EXECFILEPATH}/setout/Print.model.${LABELI}.MPI${MPPWIDTH}.log
```
