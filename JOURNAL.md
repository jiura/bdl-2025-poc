# 11/01/2026

Clonei o repositório do btcd e comecei a estudar o que o código da função original faz.

O estudo, no caso, consistiu não apenas na leitura, mas também em interações com o código,
copiando a função original e iniciando as adaptações necessárias no decorrer da função.

Aproveitei para iniciar o detalhamento dos campos necessários para a interface BlockCtx.

# 13/01/2026

Adaptei algumas das outras funções que são utilizadas pela checkConnectBlock(). A maioria
era de uso exclusivo da mesma, então apenas substituí as existentes. No entanto, esbarrei
em uma um pouco mais complicada, que depende de um struct que implementa uma interface um
pouco grande.

A função é func `(b *BlockChain) thresholdState()` (do arquivo *thresholdstate.go*). Um dos
parâmetros que ela recebe precisa implementar a interface `thresholdConditionChecker` (do
mesmo arquivo). A função atual utiliza o tipo `deploymentChecker` (do arquivo *versionbits.go*),
que atende aos requisitos.

Pondero atualmente se devo recriar o tipo `deploymentChecker` em um novo `deploymentCheckerCtx`
(ou algo do tipo), o que implicará em recriar todas as implementações necessárias para a interface
citada, ou se devo adaptar o tipo atual para servir também à nova função.

Por um lado, adaptar o serviço atual parece mais prático e limpo inicialmente, já que consistiria em
menos código adicionado e apenas algumas alterações para permitir que um `BlockCtx` seja utilizado em
vez de um `*BlockChain`. No entanto, isso dependeria de uma coexistência de ambos os campos na mesma
estrutura, o que dependeria de um controle por parte do programador, e não do compilador, para definir
qual deveria ser usado em que ocasião. Talvez utilizar `chain == nil` (onde `chain` é um `*BlockChain`)
e, a partir disso, determinar qual opção deverá ser utilizada.

Essa opção não me parece tão agradável, já que cria maior complexidade desnecessária a desenvolvimentos
futuros que envolvam essa estrutura, sendo uma definição arbitrária desse tipo um tanto insegura em relação
à alternativa.

Por outro lado há a opção que mais me apetece, recriar tudo. O que me causa desconforto com essa é a quantidade
de código a mais adicionado, mas me parece um sacrifício válido a ser realizado em prol de maior garantia de
funcionamento do código e uma experiência de desenvolvimento menos desagradável para futuros contribuidores.

Encerrei por hoje para refletir mais e tentar encontrar a melhor opção, quiçá uma terceira.

# 15/01/2026

Acabei optando por recriar toda a implementação da interface para um novo tipo.

Por enquanto, mantive todas as alterações que estava fazendo no mesmo arquivo, com o fim de evitar criar
confusão demais no meu mapa mental do projeto, que ainda não conheço tão bem. Pretendo dividir melhor e buscar
os arquivos corretos para cada nova implementação que criei futuramente.

Segui com as adaptações necessárias na função e parei durante a criação de uma nova `calcSequenceLock()`.

# 17/01/2026

Fiz bobagem com o git e perdi tudo o que estava fazendo. Não havia committado nenhuma alteração e decidi fazer
checkout para a *master* para verificar algumas configurações e rodar testes. Percebi alterações na *master* e
fiz um `git restore .`, imaginando que havia alterado algo na *master* sem querer e pretendendo testar sem minhas
modificações. Quando retornei para minha branch de desenvolvimento, todas as alterações haviam sido perdidas.

Tentei, em vão, alguns comandos diferentes do git para ver se conseguia recuperar.

Por fim, recomecei o trabalho.

# 18/01/2026

Aproveitei que precisei re-escrever para tentar me planejar melhor, agora já com o pouco conhecimento que adquiri da
última tentativa. Consegui compreender melhor o que [este PR](https://github.com/btcsuite/btcd/pull/1931) fez e segui
com o refactor, dessa vez utilizando a interface *ChainCtx* e me baseando no que já havia sido desenvolvido para ela
como exemplo do que fazer.

Aproveitei, também, para já realizar as modificações necessárias nos arquivos necessários dessa vez, agora que já
conheço um pouco melhor onde cada coisa deveria estar.

Busquei adaptar em todas as localizações necessárias quando alterando alguma função já existente, inclusive em testes.

# 19/01/2026

Não consegui assistir à apresentação do btcd ao vivo, mas comecei a assistir à gravação que foi liberada hoje. Parei em 27:57,
pretendendo terminar amanhã.

Além disso, segui com as adaptações necessárias para a função. São muitas modificações necessárias em muitos lugares diferentes,
e estou tentando garantir o mínimo possível de inconsistências, visando pouco trabalho referente a correções em outros pontos
do projeto no futuro.

# 20/01/2026

Conversei no trabalho pra conseguir pegar um pouco hoje na parte da tarde, dada que a apresentação do Checkpoint Dia 1 será às
20:40 do dia presente. Graças a isso, consegui evoluir um pouco mais nas adaptações.

Aproveitei para me organizar melhor e subi tudo o que estava pendente para evitar mais desastres com o git, principalmente no meu
fork do btcd.

Consegui finalizar as adaptações à função principal e iniciei as correções de referências que ficaram incorretas após as alterações.
Cheguei em um ponto no qual percebi que precisaria criar muitas novas funções na interface ChainCtx, mas por ora aparentemente já fiz
as alterações necessárias apenas para o refactor da função `checkConnectBlock`. Fiquei em dúvida se devo continuar refatorando, para
maximizar o uso das novas interfaces, ou se já é o bastante.

Preciso ainda analisar se as implementações da interface ChainCtx() criadas estão no arquivo mais condizente, ou se devo mover elas para
outro lugar.

Tentei adiantar o máximo possível antes do horário do Checkpoint, então ainda não terminei de assistir à gravação da apresentação sobre
btcd.
