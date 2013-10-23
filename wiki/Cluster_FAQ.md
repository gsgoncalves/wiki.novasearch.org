---
title: Cluster/FAQ
permalink: wiki/Cluster/FAQ/
layout: wiki
---

### Qual a configuração do cluster?

Tem 5 máquinas:

-   adriadne: 193.136.122.72:12034
-   arthur
-   cobb
-   mal
-   saito

A adriadne é o ponto de entrada e deve ser só para servir sessões X,
compilar e pequenos testes. As outras 4 máquinas são para tarefas \*as\*
exigentes. Cada máquina tem 6 cores, 32GB e 2 GPUs, e oferece 5.5 TFlops
(float)/ 1.4TFlop (double).

### Como acedo ao cluster?

193.136.122.72:12034 para a adriadne.

Para aceder aos outros tem de ser a partir da adriadne com o nome da
máquina, e.g., ssh saito

### Quero executar 1000 instâncias do meu programa. Posso usar a adriadne?

!!!NÃO!!! Essa máquina não deve correr simulações porque é partilhada
por todos e serve as sessões X.

### Ok. Então "onde" executo as 1000 instâncias do meu programa?

Nas outras 4 máquinas clientes.

### ok. Então "como" executo as 1000 instâncias do meu programa?

Com o condor ou com o MapReduce.

### Preciso de instalar uma biblioteca hoje!

### Não gosto de poluir os "includes" do meu imaculado pc!

Instalem a biblioteca na vossa home e alterem no -I -L do gcc. No Java
ou Python é a mesma coisa: alteram o search paths do
compilador/interpretador.

### Mas preciso mesmo que a biblioteca fique instalada no ambiente geral.

Peçam aos sudoers.

### Quais as bibliotecas/compiladores instalados?

\- gcc 4.7 - Java 1.6 - Java 1.7 - Armadillo - Freeling - Apache ?? -
OpenCV2.? - OpenMPI - ...

### Pertenço ao grupo dos "toddlers". Tenho acesso ao quê?

Full access à vossa home, que está replicada em todas as máquinas.
Reading access à pasta local /wamse/. do adriadne. Full access à pasta
local /tmp/. de todas as máquinas.

### Pertenço ao grupo dos "halfway". Tenho acesso ao quê?

Full access à vossa home, que está replicada em todas as máquinas. Full
access à pasta local /wamse/. do adriadne. Excepto a
/wamse/collections/. Full access à pasta local /tmp/. de todas as
máquinas.

### Quero pertençer ao grupo "root". O que posso fazer?

Crescer.

### Não gosto do Gnome. Posso usar outro?

Sim, escolham no servidor de X.

### Não gosto do Ubuntu. Posso usar outro SO?

Claro. No vosso portátil. Não no cluster.

### Não gosto do Ubuntu. Posso ter uma VM com SO?

Claro. No vosso portátil. Não no cluster.