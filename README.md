# Containers

Repositório para alocar arquivos de definição de containers (Singularity, Docker etc).

## Uso

Na máquina Egeon, utilize o `singularity` para construir o container. Por exemplo:

```
module load singularity
singularity build --fakeroot arquivo.sif arquivo.def
```

No final do processo, será gerado o arquivo `arquivo.sif`.

* Arquivo `BAM_V1.2.1.def`: arquivo de definição do singularity para compilar o modelo BAM_V1.2.1 utilizando o GCC 4.8.5 e MPICH 3.3 (utiliza o Ubuntu 18.04 como base);
* Arquivo `BAM_V1.2.0.def`: arquivo de definição do singularity para compilar o modelo BAM_V1.2.0 utilizando o GCC 4.8.5 e MPICH 3.3 (utiliza o Ubuntu 18.04 como base);
* Arquivo `SCANTEC_V2.0.0.def`: arquivo de definição do singularity para compilar o SCANTEC V2.0.0 utilizando o GCC 9.0 (utiliza o Ubuntu 20.04 como base);
* Arquivo `ubuntu_Eval_V1.0.0.def`: arquivo de definição do singularity para compilar o readDiag utilizando o GCC 4.8.5 (utiliza o Ubuntu 18.04 como base), incluindo o Miniconda e um ambiente chamado readDiag.
