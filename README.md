# INFO7015_jellyfish
INFO7015 - Reproduzindo Jellyfish (TP1)

## Resumo dos métodos e resultados do artigo original

Os autores avaliaram o jellyfish sobre três pontos de vista: eficiência, flexibilidade e resiliência à falhas. A eficiência foi avaliada através da comparação da interconexão conhecida como fat-tree. Nesse caso, eles constataram que o jellyfish suporta 25% mais servidores que o fat-tree. Consequentemente, o througput também aumenta com o número de servidores, sendo que o jellyfish apresenta um througput 27% maior que o fat-tree.

Do ponto de vista da flexibilidade, o jellyfish permite que qualquer número de racks seja conectado em rede de maneira eficiente, contrastando com outras soluções, que dependem da disponibilidade de portas livres nos switches. A expansão do jellyfish se dá pelo cabeamento das portas que estão sendo adicionadas à rede, o que torna esse processo mais simples. Em relação à resiliência à falhas, o jellyfish apresenta uma boa redundância de caminhos, aproximando-se de um grafo conexo. Comparado ao fat-tree, ele apresenta um throughput decrescente à medida que aumenta o número de links com falhas, porém de maneira mais lenta que o fat-tree.

A Figura 9 do artigo representa os resultados do uso dos mecanismos de controle de congestionamento – ECMP_8, ECMP_64 e 8_shortest Path. Observa-se que no ECMP, mais de 55% dos links são utilizados por não menos que 2 caminhos, enquanto que no 8_shortest Path, esse percentual cai para 6%. Isso demonstra que o mecanismo 8_shortest Path aproveita melhor a diversidade de caminhos oferecida.

![](https://github.com/agnaldosb/INFO7015_jellyfish/blob/master/figures/Figura9.png&s=200)

A Tabela 1 do artigo apresenta os resultados das simulações de troca de pacotes realizadas, permitindo a comparação do desempenho do jellyfish com o fat-tree. Observa-se que o jellyfish apresenta uma pequena melhora em relação ao fat-tree. Porém, essa pequena melhora tem um peso maior se considerarmos que o jellyfish foi testado com 780 servidores, enquanto o fat-tree, 686 servidores. Tem-se uma diferença de 13,7% no número de servidores a favor do jellyfish.

##### Inserir Tabela 1 original aqui

## Questões respondidas

### Que problema a Jellyfish estava tentando resolver? 

O jellyfish foi apresentado como uma solução para a expansão de datacenters. Para isso, os autores propuseram a adoção de uma topologia baseada em grafos randômicos, indo de encontro às soluções existentes à época, que trabalhavam com estruturas de redes rígidas.

### Qual era o estado da arte no momento em que o artigo foi publicado?

Em 2012, quando o Jellyfish foi apresentado, as atenções estavam voltadas para topologias e roteamento. Nesse contexto, havia o folded-Clos (ou fat-tree) [6, 18, 35], o uso de servidores para encaminhamento [19, 20, 45] e o uso de redes óticas. A expansão dos datacenters demandava adicionar servidores, o que exigia a substituição de dispositivos de rede, além de modificações no cabeamento. Algumas soluções então existentes permitiam a expansão dos datacenters com um alto custo, como era o caso do MDCube [45], enquanto outras viabilizavam essa mudança para um tamanho pré-definido, como era o caso do DCell e BCube [19, 20]. Propostas como o Scafida [21] e SWDC [41] já trabalhavam com randomicidade, tal como o Jellyfish, porém demandavam uma correlação entre as arestas dos grafos.

Uma outra proposta existente à época era o LEGUP [14], que trabalhava na melhoria do uso do fat-tree. Porém, ele era limitado pela estrutura da rede, sendo que a expansão do datacenter ficava condicionada à disponibilidade de portas livres. A proposta REWIRE [13] buscava topologias de alta capacidade com um determinado custo, levando em conta o custo da variação do comprimento do cabeamento.

### Você estendeu o experimento de alguma forma? 

Não estendi. Realizei o experimento em parte, não tendo sucesso na implementação do algoritimo fat-tree.

## Reprodução da Figura 9 e da Tabela 1 

Para reprodução da Figura 9 e da Tabela 1, foi utilizado como referência o trabalho disponível em https://github.com/aghalayini/CS244_jellyfish. Esse pesquisador implementou a Figura 9, bem como parte dos testes constantes da Tabela 1, porém apenas para o jellyfish.

Foi necessário realizar algumas modificações no trabalho citado, a fim de adequá-lo aos requisitos da disciplina. As dificuldades em torno desse experimento foram diversas:
- Os recursos de hardware disponíveis não são suficientes para testar com o mesmo número de servidores que os autores do artigo utilizaram. Dessa forma, reduzi os testes a 10 servidores.
- Os testes com o protocolo TCP com 1 fluxo funcionaram normalmente. Porém, quando foi modificado para 8 fluxos, os resultados sofreram algumas variações, quando foi necessário avaliar os arquivos de resultados isoladamente.
- No caso do protocolo MPTCP, quando são testados 8 fluxos, ocorreu a mesma situação do protocolo TCP, quando foi necessário avaliar os arquivos de resultados isoladamente.
- Não consegui implementar o algoritimo fat-tree a fim de viabilizar os testes e posteriores comparações com o jellyfish.

### Resultados obtidos

#### Inserir a Figura 9 obtida aqui!

Observa-se que a Figura 9 obtida assemelha-se em grande medida àquela existente no artigo original.

#### Inserir a Tabela 1 obtida aqui!

Os resultados obtidos encontram-se na tabela abaixo. Comparando-se essa tabela com àquela do artigo, observa-se que apenas o resultado do teste do protocolo TCP com 1 fluxo e usando o 8_shortest se aproximou-se do resultado do artigo. Os demais ficaram muito distantes dos valores do artigo. As causas prováveis para essa disparidade de valores pode estar no número de servidores utilizados nos testes, por exemplo.


### Instalação e execução do experimento

Para realização do experimento, foi feita a configuração dos parâmetros relativos à topologia do Jellyfish, que pode ser estabelecida no arquivo build_topology.py da seguinte forma:

### Explicar a geração da figura e a troca dos parâmetros para as simulações

Para executar o experimento, seguir seguintes passos:

1. Assegure-se de que o python2 e o pip2 estão instalados.
2. Executar “pip2 install matplotlib”
3. Executar “pip2 install networkx”
4. Executar “git clone git://github.com/mininet/mininet”
5. Executar “mininet/util/install.sh -a”
7. Usando o comando “ls”, você constatará diversas pastas e arquivos.
8. Executar “git clone https://github.com/agnaldosb/INFO7015_jellyfish”
9. Executar “cd INFO7015_jellyfish/pox/ext/”

#### Geração da Figura 9

Para gerar a figura 9, após a configuração para execução do experimento conforme passos acima, proceder da seguinte forma:

-  Executar “python2 ./graph.py”: Você observará dois arquivos .svg no diretório corrente. O arquivo 1.svg representa a Figura 9 do artigo, enquanto o arquivo 2.svg é a forma do gráfico randomico.

#### Realização dos testes

Para executar os testes, que permitiram a elaboração de parte da Tabela 1, deve-se observar os seguintes passos:

1. Modificar os parâmetros constantes no arquivo build_topology.py, que se encontra na pasta INFO7015_jellyfish/pox/ext. Os parâmetros de interesse são os seguintes:

- (Parameters)
- r_method = '8_shortest' # 'ecmp8', 'ecmp64' or '8_shortest'
- number_of_tcp_flows = 8 # should be 1 or 8
- nx_topology = NXTopology(number_of_servers=10, switch_graph_degree=3, number_of_links=15)

3. Nos testes realizados foram usados 10 servidores. Os parãmetros que devem ser modificados conforme o teste a ser realizado são o controle de congestionamento (r_method) e o número de fluxos.
2. Executar “sudo python2 ./build_topology.py”: A simulação será realizada, sendo que os resultados do Iperf para cada servidor serão registrados no arquivo .out no diretório /ext, enquanto que os erros que surgirem serão armazenados nos arquivos .err. O throughput médio será impresso ao final dos testes. Você pode modificar os parâmetros na seção parameteres section do arquivo build_topology.py.

### Configurações do ambiente de simulação

Para realização das simulações, foi instalado o sistema operacional Linux Ubuntu, cujo kernel foi recompilado para incorporar o protocolo MPTCP. Esse sistema operacional foi instalado em uma máquina virtual no VM VirtualBox. Os experimentos foram realizados no Mininet.

#### Configurações de hardware:

- Desktop Dell Inspiron 
- Processador Intel(R) Core(TM) i5-4460S CPU @ 2.90GHz
- 64 bits
- 8GB memória RAM

#### Softwares utilizados

- Sistema operacional Linux Ubuntu - versão 16.04.5 LTS
- Kernel - versão 4.14.64MPTCP+ (recompilado para incorporar o protocolo MPTCP)
- VM VirtualBox - versão 5.2.18 r124319
- Mininet  - versão 2.3.0d4
- Iperf - versão 2.0.5
- Python - versão 2.7.12
