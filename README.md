# T3_Robos_Autonomos
Identificar pessoas no ambiente e fazer o robô se posicionar ao lado da pessoa

## Objetivo
O nosso objetivo neste terceiro trabalho é explorar conceitos de navegação e exploração em um ambiente complexo para realização de uma tarefa envolvendo um humano.

## Descrição
Este trabalho consiste em utilizar um robô móvel com um conjunto de sensores em um ambiente parcialmente desconhecido para (i) encontrar uma pessoa no ambiente e (ii) se posicionar ao lado da pessoa.

### Requisitos mínimos:
- O robô deve ser 100% autônomo; não deve ser teleoperado ou depender de comandos externos;
- O robô deve ser capaz de encontrar e identificar uma pessoa, posicionada de maneira aleatória no ambiente;
- Após encontrar a pessoa, o robô deve se posicionar ao lado dela, como se tivesse a intenção de seguí-la.

### Detalhamento:
- O trabalho deve ser desenvolvido em grupos de 3 alunos. Recomenda-se que os grupos mesclem alunos de graduação e de pós, e que os integrantes tenham variados níveis de conhecimento de programação e do ROS 2. É positivo mudar os grupos em relação aos grupos do trabalho anterior;
- Modelos de sensores e robô podem ser escolhidos pelos alunos. Os sensores devem estar montados no robô. Indica-se a utilização do Clearpath Husky A200 (seguir os tutoriais de simulação da Clearpath para o ROS2 Humble - link);
- O modelo de mundo usado na simulação deve ser o "warehouse", do pacote Clearpath Simulator (link). Fique atento para usar o branch “humble” do pacote. O mundo pode ser modificado para gerar diferentes versões com pessoas em posições diferentes. As alterações devem ser feitas para variar o grau de dificuldade da tarefa, mas realizações mais complexas são preferidas;
- O robô deve ser inicializado em uma posição aleatória do mapa;
- Ao encontrar uma pessoa, deverá haver algum tipo de sinalização que indique que a pessoa foi localizada. Em seguida, o robô deve se posicionar ao lado da pessoa encontrada (ao lado, e não à frente ou atrás);
- A tarefa é concluída uma vez que o robô chegue ao lado da pessoa. Ao finalizar a tarefa, deve ser gerado um arquivo de texto que contenha a localização aproximada onde a pessoa foi encontrada e o tempo total para que a tarefa fosse concluída. Outros tipos de log são bem-vindos;
- Não há restrição de pacotes, robôs ou sensores a serem utilizados neste trabalho. Os alunos podem escolher livremente as ferramentas que melhor se adequem à estratégia escolhida para realização da tarefa. Todas as ferramentas utilizadas devem ser documentadas e apresentadas em detalhes durante a apresentação do trabalho. De qualquer maneira, a sugestão é de que sejam seguidos os tutoriais da Clearpath para facilitar a execução. Ao final deste documento eu resumo os passos que dei para fazer os tutoriais da Clearpath funcionarem no Ubuntu 22.04.
- A solução apresentada deve ser automatizada e reproduzível. Todo código desenvolvido pelos alunos deve estar contido dentro de um pacote ROS, aberto e com link para o repositório disponibilizado. Deve existir, neste pacote, necessariamente, um arquivo launch que demonstre a resolução do problema (i.e., deve subir o Gazebo, o mundo, o robô, os nós necessários, e uma forma de visualizar a solução do problema proposto). Inclua no repositório um documento de texto (README) indicando os procedimentos de instalação e requisitos (versão OS e ROS, pacotes instalados, etc.) para que seja possível reproduzir seu trabalho;


## Questões Propostas

Usem as seguintes questões para guiar as discussões:

- Como se pode formalizar o problema a ser resolvido?
- Quais as diferentes linhas de ação?
- A solução desenvolvida é genérica ou funciona apenas em ambientes muito semelhantes?
- Como a solução se comportaria se a pessoa estivesse andando?
- Quais modificações seriam necessárias para aprimorar a solução?
- Quais outros problemas poderiam ser resolvidos com abordagem semelhante?

**Obsevação importante**: as questões acima não devem ser apresentadas como tópicos e não devem estar explícitas na sua apresentação. As questões devem ser usadas como guia, como forma de fomentar o pensamento crítico durante a preparação da apresentação.

## Observações
• O mundo em questão possui diversos modelos de objetos/obstáculos complexos que podem deixar a simulação pesada. Os grupos podem remover esses objetos do mundo caso eles inviabilizem a simulação. Porém, cada objeto (modelo de objeto) removido deve ser documentado e comentado durante a apresentação. Dê preferência para remover objetos que não alteram significativamente a execução da tarefa;

• Como o mundo utilizado possui uma área muito extensa, é recomendado que a simulação seja executada em uma velocidade maior do que a velocidade normal. Isso pode ser feito modulando o parâmetro RealTime Factor (RTF) do Gazebo, para que a simulação seja executada o tão rápido quanto possível. Por default, o RTF é limitado a 1, ou seja, 1s da simulação corresponde a 1s na vida real. Diversos parâmetros podem ser ajustados para se obter RTFs mais altos (link 1, link 2, link 3).

• Evite conversar com outros grupos sobre as estratégias de resolução deles. Em hipótese alguma compartilhe código ou arquivos, especialmente os mundos. Mantenha seu repositório fechado (privado) até a data da apresentação.


## Simulando o Clearpath Husky no mundo Warehouse
Vamos considerar o Humble já instalado. Siga o tutorial dessa página (link) para a instalação dos pacotes da Clearpath. Para evitar problemas, eu também dei o comando “sudo apt instal ros-humble-clearpath*”, com o wildcard * para baixar todos os pacotes da Clearpath. Além disso, o repositório deles aponta para um bug em relação ao pacote “clearpath_sensors”, que precisa ser instalado manualmente (link do issue com solução do problema).

Também é preciso criar um arquivo robot.yaml na pasta $HOME/clearpath (que criei manualmente). O robot.yaml do tutorial não parece funcionar direto. Ele precisa incluir o workspace que você está utilizando e precisa ser atualizado com o formato que está atualizado no repositório. Vou anexar no Classroom o arquivo que funcionou no meu computador para vocês terem de modelo.

Simplesmente rodar o launch “clearpath_gz simulation.launch.py“ não sobe o modelo do robô no rviz e não faz aparecer os TFs no rviz. Pelo menos não seguindo os passos que descrevi. Os nós de controle do robô parecem não subir, mas não me aprofundei no porquê - é preciso olhar com calma os arquivos launch. Para contornar isso, a forma mais fácil que encontrei foi rodar em outro terminal o comando “ros2 launch clearpath_viz view_robot.launch.py namespace:=a200_0000“. Assim, o rviz2
abre com os TFs encontrados e com o modelo do robô. Além disso, você pode verificar que tudo funciona usando a aba de teleoperação de dentro do Gazebo.

