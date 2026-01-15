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

# 14/01/2026

TBA
