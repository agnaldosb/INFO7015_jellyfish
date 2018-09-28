# INFO7015_jellyfish
INFO7015 - Reproduzindo Jellyfish (TP1)

## Resumo dos métodos e resultados do artigo original

Os autores avaliaram o jellyfish sobre três pontos de vista: eficiência, flexibilidade e resiliência à falhas. A eficiência foi avaliada através da comparação da interconexão conhecida como fat-tree. Nesse caso, eles constataram que o jellyfish suporta 25% mais servidores que o fat-tree. Consequentemente, o througput também aumenta com o número de servidores, sendo que o jellyfish apresenta um througput 27% maior que o fat-tree.

Do ponto de vista da flexibilidade, o jellyfish permite que qualquer número de racks seja conectado em rede de maneira eficiente, contrastando com outras soluções, que dependem da disponibilidade de portas livres nos switches. A expansão do jellyfish se dá pelo cabeamento das portas que estão sendo adicionadas à rede, o que torna esse processo mais simples. Em relação à resiliência à falhas, o jellyfish apresenta uma boa redundância de caminhos, aproximando-se de um grafo conexo. Comparado ao fat-tree, ele apresenta um throughput decrescente à medida que aumenta o número de links com falhas, porém de maneira mais lenta que o fat-tree.

A Figura 9 do artigo representa os resultados do uso dos mecanismos de controle de congestionamento – ECMP_8, ECMP_64 e 8\_shortest Path. Observa-se que no ECMP, mais de 55% dos links são utilizados por não menos que 2 caminhos, enquanto que no 8\_shortest Path, esse percentual cai para 6%. Isso demonstra que o mecanismo 8_shortest Path aproveita melhor a diversidade de caminhos oferecida.

![](https://github.com/agnaldosb/INFO7015_jellyfish/blob/master/figures/Figura9.png)

A Tabela 1 do artigo apresenta os resultados das simulações de troca de pacotes realizadas, permitindo a comparação do desempenho do jellyfish com o fat-tree. Observa-se que o jellyfish apresenta uma pequena melhora em relação ao fat-tree. Porém, essa melhora tem um peso maior se considerarmos que o jellyfish foi testado com 780 servidores, enquanto o fat-tree, 686 servidores. Tem-se uma diferença de 13,7% no número de servidores a favor do jellyfish.

![](https://github.com/agnaldosb/INFO7015_jellyfish/blob/master/figures/Tabela1.png)

## Questões respondidas

### Que problema a Jellyfish estava tentando resolver? 

O jellyfish foi apresentado como uma solução para a expansão de datacenters. Para isso, os autores propuseram a adoção de uma topologia baseada em grafos randômicos, indo de encontro às soluções existentes à época, que trabalhavam com estruturas de redes rígidas.

### Qual era o estado da arte no momento em que o artigo foi publicado?

Em 2012, quando o Jellyfish foi apresentado, as atenções estavam voltadas para topologias e roteamento. Nesse contexto, havia o folded-Clos (ou fat-tree) [6, 18, 35], o uso de servidores para encaminhamento [19, 20, 45] e o uso de redes óticas. A expansão dos datacenters demandava adicionar servidores, o que exigia a substituição de dispositivos de rede, além de modificações no cabeamento. Algumas soluções então existentes permitiam a expansão dos datacenters com um alto custo, como era o caso do MDCube [45], enquanto outras viabilizavam essa mudança para um tamanho pré-definido, como era o caso do DCell e BCube [19, 20]. Propostas como o Scafida [21] e SWDC [41] já trabalhavam com randomicidade, tal como o Jellyfish, porém demandavam uma correlação entre as arestas dos grafos.

Uma outra proposta existente à época era o LEGUP [14], que trabalhava na melhoria do uso do fat-tree. Porém, ele era limitado pela estrutura da rede, sendo que a expansão do datacenter ficava condicionada à disponibilidade de portas livres. A proposta REWIRE [13] buscava topologias de alta capacidade com um determinado custo, levando em conta o custo da variação do comprimento do cabeamento.

### Você estendeu o experimento de alguma forma? 

Não estendi. Realizei o experimento em parte, não tendo sucesso na implementação do algoritimo fat-tree.

## Reprodução da Figura 9 e da Tabela 1 

Para reprodução da Figura 9 e da Tabela 1, foi utilizado como referência o trabalho disponível em https://github.com/aghalayini/CS244_jellyfish. O autor implementou a Figura 9, bem como parte dos testes constantes da Tabela 1, porém apenas para o jellyfish.

Foi necessário realizar algumas modificações no trabalho citado, a fim de adequá-lo aos requisitos da disciplina. As dificuldades em torno desse experimento foram diversas:

- Os recursos de hardware disponíveis não são suficientes para testar com o mesmo número de servidores que os autores do artigo utilizaram. Dessa forma, reduzi os testes a 10 servidores.

- Os testes com o protocolo TCP com 1 fluxo funcionaram normalmente. Porém, quando foi modificado para 8 fluxos, os resultados sofreram algumas variações, quando foi necessário avaliar os arquivos de resultados isoladamente.
- No caso do protocolo MPTCP, quando são testados 8 fluxos, ocorreu a mesma situação do protocolo TCP, quando foi necessário avaliar os arquivos de resultados isoladamente.
- Não consegui implementar o algoritimo fat-tree a fim de viabilizar os testes e posteriores comparações com o jellyfish.

### Instalação e execução do experimento

Para executar o experimento, seguir seguintes passos:

- Assegure-se de que o python2 e o pip2 estão instalados.
- Executar “pip2 install matplotlib”
- Executar “pip2 install networkx”
- Executar “git clone git://github.com/mininet/mininet”
- Executar “mininet/util/install.sh -a”
- Usando o comando “ls”, você constatará diversas pastas e arquivos.
- Executar “git clone https://github.com/agnaldosb/INFO7015_jellyfish”
- Executar “cd INFO7015_jellyfish/pox/ext/”
- A partir desse momeno, os trabalhos serão realizdos a partir desta pasta.

### Resultados obtidos

#### Reprodução da Figura 9

Para gerar a Figura 9, após a configuração para execução do experimento conforme passos acima,  executar no terminal o comado “python2 ./graph.py”. Serão criados dois arquivos .svg no diretório corrente. O arquivo 1.svg representa a Figura 9 do artigo, enquanto o arquivo 2.svg é a forma do gráfico randomico.

Observa-se que a Figura 9 obtida assemelha-se em grande medida àquela existente no artigo original. A quantidade de servidores utilizados para cálcula foi a mesma daquela utilizada no artigo.

![](https://github.com/agnaldosb/INFO7015_jellyfish/blob/master/figures/Figure9_results.png)

#### Reprodução da Tabela 1

Os testes envolvendo a reprodução da Tabela 1 do artigo original foram feitos com diversos cenários, onde foi necessário modificar o roteamento - ecmp8 ou 8\_shortest - e a quantidade de fluxos - 1 ou 8. Os demais parâmetros foram mantidos inalterados. Esses  parâmetros são modificados no arquivo 'build\_topology.py', que se encontra na pasta INFO7015_jellyfish/pox/ext. Os parâmetros de interesse são os seguintes:

	- r_method = '8_shortest' # 'ecmp8', 'ecmp64' or '8_shortest'
	- number_of_tcp_flows = 8 # should be 1 or 8
	
- Para obtenção dos resultados da tabela 1, foram realizadas 5 simulações para cada cenário e feita a média aritmética simples dos valores obtidos.
- Foram usados 10 servidores para avaliação, haja vista o tempo de teste tornar-se demasiadamente longo à medida que a quantidade de servidores é aumentada.
- Para executar os testes, deve-se excecutar o comando “sudo python2 ./build\_topology.py”. Os resultados do Iperf para cada servidor serão registrados no arquivo .out no diretório /ext, enquanto que os erros que surgirem serão armazenados nos arquivos .err. O throughput médio será impresso ao final dos testes. 

Os resultados obtidos encontram-se na tabela abaixo. Comparando-se essa tabela com àquela do artigo, observa-se que apenas o resultado do teste do protocolo TCP com 1 fluxo e usando o 8_shortest se aproximou-se do resultado do artigo. Os demais ficaram muito distantes dos valores do artigo. As causas prováveis para essa disparidade de valores pode estar no número de servidores utilizados nos testes, por exemplo.

![](https://github.com/agnaldosb/INFO7015_jellyfish/blob/master/figures/Tabela1_results.png)

Essa distinção de resultados era esperada, haja vista que o cenário avaliado não foi exatamente o mesmo do artigo original, especialmente em relação à quantidade de servidores. Adicionalmente, nos testes envolvendo 8 fluxos e 8 subfluxos, os resultados apresentaram muitos erros em relação ao troughput. Após pesquisa, foi verificado que o Iperf não lidava adequadamente com diversos fluxos. Em geral, ele tratava adequadamente até 5 fluxos, às vezes excedendo esse número. Porém, em momento algum o Iperf apresentou resultados satisfatórios para 8 fluxos em um mesmo teste.

## Configurações do ambiente de simulação

Para realização das simulações, foi instalado o sistema operacional Linux Ubuntu, cujo kernel foi recompilado para incorporar o protocolo MPTCP. Esse sistema operacional foi instalado em uma máquina virtual no VM VirtualBox. Os experimentos foram realizados no Mininet.

No caso da Tabela 1, que demandava testes com os protocolos TCP e MPTCP, foi necessário configurar o kernel para habilitar o MPTCP.

### Configurações de hardware

- Desktop Dell Inspiron 
- Processador Intel(R) Core(TM) i5-4460S CPU @ 2.90GHz
- 64 bits
- 8GB memória RAM

### Softwares utilizados

- Sistema operacional Linux Ubuntu - versão 16.04.5 LTS
- Kernel - versão 4.14.64MPTCP+ (recompilado para incorporar o protocolo MPTCP)
- VM VirtualBox - versão 5.2.18 r124319
- Mininet  - versão 2.3.0d4
- Iperf - versão 2.0.5
- Python - versão 2.7.12
