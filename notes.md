#### 11/02/2024

Curso de Refatoração em PHP: boas práticas no seu código

@01-Primeiros passos

@@01
Apresentação

[00:00] E aí, pessoal? Sejam muito bem-vindos à Alura, eu sou o Vinícius Dias e vou guiar vocês em um treinamento onde vamos falar um pouco sobre refatoração e nós vamos utilizar a linguagem de programação PHP para isso. O que eu espero que você saiba de PHP antes de continuar comigo?
[00:16] Eu espero que você tenha, pelo menos, uma boa base de orientação a objetos, porque a maioria das técnicas de refatoração envolve orientação a objetos. Eu espero que você tenha pelo menos um pouco de familiaridade com o Composer, porque vamos utilizar o Composer para instalar uma biblioteca no meio desse caminho.

[00:32] Se você conhecer sobre testes, isso é muito bom e vai te adiantar em alguns momentos, porque temos testes neste projeto. Mas, se você ainda não conhece, não se preocupe, pode fazer este treinamento e estudar sobre testes depois. E o treinamento de Composer, aqui da Alura, fala um pouco sobre buscar dados na internet, inclusive temos um outro curso específico sobre web scraping.

[00:53] Nós vamos utilizar um pouco disso neste treinamento, porque refatoração é o processo de melhorar um pouco o código, e nada melhor do que melhorar o código de um código real, de um projeto real. Então eu vou utilizar aqui um projeto que inclusive está abandonado, ou seja, é um projeto relativamente antigo. Deste projeto, partindo deste projeto, nós vamos mudar, vamos modificar, vamos mexer no código.

[01:15] Deixa eu te apresentar esse projeto antes mesmo de começarmos o curso para você entender o código onde vamos mexer. Dando uma olhada neste projeto, o que ele faz? Ele é um Googlecrawler, ou seja, ele vai analisando, ele pega uma página do Google que você pesquisou, por exemplo, uma busca por teste.

[01:34] Ele vai retornar vários resultados e esse código vai resultado a resultado, pegando o link e a descrição desse resultado para armazenar. Em cima disso podemos fazer várias coisas, por exemplo, esse código já foi usado, em produção, para realizar análise de SEO de alguns concorrentes inclusive. Isso é bastante interessante, é um código que já foi usado em produção.

[01:56] Mas, continuando, nós temos, começando aqui, um termo de busca. Ele simplesmente recebe uma string e devolve uma string, ele pode ser representado como uma string.

[02:05] Aqui, nessa implementação, ele simplesmente remove espaços, ele encoda como se fosse uma URL. Esse termo de busca é utilizado pelo nosso crawler. Então aqui, no nosso "Crawler.php", nós recebemos esse termo de busca, podemos utilizar algum proxy, porque quando você faz muitas requisições para o Google, ele identifica que você está utilizando algum tipo de robô, então eu não vou deixar mais você usar sem desbloquear um captcha.

[02:29] Então nós podemos utilizar alguns proxies para fazer essas requisições. Já temos até alguns proxies implementados como esse antigo.

[02:38] Esses proxies não estão com a implementação atualizada, mas tudo bem. E podemos fazer requisições para mais de um domínio do Google, caso você queira fazer uma busca para um país específico, inclusive podemos passar o código do país.

[02:50] Mas, enfim, com isso tudo, o que ele faz é armazenar essas informações que recebeu no construtor e buscar os resultados. Então ele faz o crawl, ou seja, ele faz o parse desse resultado, ele rasteja nesse resultado, vai fazendo o parse, e no final ele devolve uma lista de resultados.

[03:08] Essa lista de resultados nada mais é do que um vetor, que é como se fosse um array, só que bem mais performático, tem treinamentos aqui sobre esse assunto na Alura também. Mas, com esse vetor, ele basicamente vai adicionando resultados. Dessa forma eu garanto que essa lista sempre terá resultados. Instâncias da classe? Resultado e nada além disso.

[03:28] Então aqui temos uma URL, um título e uma descrição. Repare que esse código é antigo, ele ainda não usava nenhum o PHP 74, nem o PHP 8, então as propriedades ainda não eram tipadas, nós podemos fazer isso fora desse treinamento, vale a pena estudar sobre isso.

[03:43] Mas, enfim, continuando aqui, nós temos algumas exceções que podem ser lançadas quando o resultado do Google vem de um formato inválido, que não sabemos parsear, quando o resultado em si, é inválido, ou seja, não é um resultado em texto, é uma imagem, uma sugestão do Google ou alguma coisa assim, ou quando passamos uma URL em um formato inválido.

[04:03] Então esses são alguns possíveis erros que o nosso código lança em determinados momentos e aqui está a implementação dos proxies.

[04:09] Aqui nós temos aquela interface que o nosso crawler espera e ela sabe fazer uma requisição e devolver uma resposta HTTP, e fazer o parse de uma URL. Aqui nós temos uma implementação sem usar proxy nenhum.

[04:21] Fazendo a requisição diretamente, sem fazer nenhuma modificação, e fazendo o parse da URL do Google, do resultado do Google. Com isso já temos conhecimento suficiente para começar a modificar esse código. Para modificar, vamos acabar rodando alguns testes. De novo, se você não entende sobre testes, não tem problema, você não precisa saber agora.

[04:41] Mas, basicamente, são códigos que vão executar o nosso código para garantir que o resultado é o esperado. Conforme vamos mudando o nosso código, precisaremos mudar um pouco desses testes também. Tendo feito essa introdução do código que vamos ver, esse código que você já deu uma olhada, que dá para melhorar bastante, vamos usar como referência um livro muito famoso.

[05:03] "Refactoring: Improving the design of existing code". Ou seja, refatorando: melhorando o design de código existente. Esse livro tem uma segunda edição, mas eu li a primeira edição dele e ele é uma leitura um pouco difícil, vamos dizer maçante. É difícil de ler esse livro, eu falo mais disso no final.

[05:25] Uma outra referência bem interessante é o "refactoring.guru". De novo, no final do treinamento eu vou deixar essas referências para você, mas para você já fica situado, se quiser pesquisar uma coisa ou outra durante o treinamento.

[05:36] Mas falei bastante, já te apresentei sobre vários termos aqui, tanto te apresentei ao projeto quanto às referências utilizadas, agora vamos finalmente botar a mão na massa, começar a entender o que é essa tal de refatoração e como aplicar.

@@02
Projeto inicial do treinamento

Baixe aqui o ZIP com os arquivos para o projeto inicial deste treinamento, necessário para a continuidade do mesmo.

https://github.com/alura-cursos/google-crawler/archive/refs/tags/projeto-inicial.zip

@@03
O que é refatoração

[00:00] E aí, pessoal? Bem-vindos de volta. Vamos, neste vídeo, entender o que é esse processo de refatoração. Para isso, como eu comentei na introdução, vamos utilizar um projeto real, um projeto que já existe, um projeto relativamente antigo e que tem muito código a ser melhorado. Para você pegar esse projeto você vai baixar, do primeiro exercício deste treinamento, e com o Composer você vai simplesmente executar composer install.
[00:25] Eu já rodei, então aqui já está tudo configurado, eu não preciso fazer de novo, mas você vai fazer essa instalação, porque tem alguns pacotes como dependência desse projeto. Mas, feito isso, estamos prontos para executar tudo aqui. Inclusive, esse projeto, embora ele seja um pouco antigo, ele tem alguns testes.

[00:42] Enquanto vamos mexendo, eu posso deixar aqui os testes rodando. Repare que, por ser um projeto antigo, ele inclusive tem alguns testes incompletos, alguns testes que ele vai pular, que são testes que poderiam acabar falhando, enfim, tem alguns problemas nessa suíte de testes, mas vemos que eles estão passando.

[01:01] Mas vamos lá, vamos continuar para o projeto real. Aqui, em "src", a classe principal, a classe que faz a maior parte das tarefas deste projeto, é essa class Crawler.

[01:12] Nessa classe nós temos um problema que está me incomodando: eu não quero mudar a funcionalidade do meu código, mas eu quero melhorar ele. Eu quero fazer com que o meu código faça mais sentido. Então, neste ponto, o que eu quero fazer? Na hora que eu construo uma classe crawler eu não quero precisar passar tantos parâmetros, eu quero passar só o que é necessário para a funcionalidade em si, da classe como um todo.

[01:37] Para a classe como um todo, nós precisamos dessa dependência de proxy. Agora, eu construí esse crawler, que faz uma busca no Google. A partir deste crawler eu quero poder fazer várias buscas no Google. Então o termo de busca, o domínio, o código do país, esses detalhes, eu não quero receber aqui. Então vamos receber no outro método, no método que efetivamente faz a busca.

[02:00] Vamos mover os parâmetros daqui para aquele outro método. Vamos nessa, no método getResults(), ou seja, que recupera os resultados, vamos receber uma (SearchTermInterface $searchTerm, ), ou seja, um termo de busca, vamos receber uma string $googleDomain, e vamos receber o string $countryCode). Vou quebrar a linha, para isso ficar um pouco mais legível.

[02:26] Repare, como esse código foi feito há algum tempo, ainda temos algumas anotações, coisas que talvez nem seriam necessárias, mas vamos lá. Então eu estou recebendo os parâmetros que, repare, não estão sendo usados, mas eu vou dar um "Alt + Enter", e atualizar os comentários de código. Vamos nessa, continuar.

[02:44] Eu não vou mais receber esses parâmetros aqui, não vou mais receber aqui. Não preciso dessa parte $searchTera e (stripos($googleDomain, needle 'googl'.') eu vou validar no ResultList. Vamos lá, essa outra parte eu não preciso e essa outra eu também não preciso.

[03:00] Eu já tenho uma diminuição no meu construtor, uma diminuição de código, eu não tenho mais lógica no meu construtor, mas ainda dá para melhorar ele. Vamos avançar, eu quero pegar o searchTerm, então vamos ver onde que eu utilizava isso, que eu vou remover. Essa parte não precisa mais ser campos da nossa classe.

[03:22] Vamos ver onde eu uso elas. Se eu não me engano, eu uso na hora de pegar a URL. No getGoogleUrl(). Repare que eu estou vendo onde essa variável é usada e vou passar para lá. Então aqui eu vou passar, ele espera getGoogleUrl, eu vou passar o ($searchTerm).

[03:38] Porque aqui embaixo ele não vai mais usar o search term da classe, ele vai utilizar esse parâmetro. Então getGoogleUrl(string $searchTerm). Não é uma string que ele recebe, (SearchTermInterface $searchTerm). Agora sim. Essa SearchTermInterface tem aquele método __toString.

[04:00] Só para você dar uma olhada, ele tem um __toString, então ele pode ser representado como uma string. Continuando, ele também vai precisar de um Google domain, vamos receber aqui (SearchTermInterface, $searchTerm, string $googleDomain). E vamos nessa. Não preciso deste this mais, vai pegar direto o $googleDomain.

[04:17] Beleza, o country code, a mesma coisa, também vamos receber o string $countryCode. E vamos utilizar aqui embaixo, diretamente, sem acessar, sem o this, da propriedade da classe.

[04:32] Vamos remover também o country code, ele já foi removido, show de bola. Vamos lá, estou passando aqui o ($searchTerm), preciso passar o $googleDomain e preciso passar o $countryCode.

[04:44] Agora, teoricamente, está tudo certo. Vamos ver se utilizamos isso em outro lugar? Se eu fizer a busca por "search term", não estou utilizando em nenhum outro lugar. Repare que eu tinha uma propriedade criada no construtor para eu utilizar em um único método. Isso não era muito útil, concorda comigo?

[05:02] Agora que fizemos essa refatoração, o código deve se comportar da mesma forma. Então, o que é o conceito de refatoração, para finalizarmos esse vídeo? A ideia de refatorar um código é alterar o design dele, alterar a implementação, mas sem que a funcionalidade em si mude. Ou seja, o meu código, ele continua fazendo a mesma coisa, ele continua acessando o Google, ele continua recebendo os mesmos parâmetros para fazer a mesma tarefa.

[05:30] Ele acessa o Google, processa uma lista de resultados e devolve. Nesse cenário específico eu fiz algo que não é tão comum, eu mudei a assinatura dos métodos. O meu construtor agora está diferente, eu recebo só um parâmetro.

[05:42] Esse método agora recebe mais parâmetros. Então quando estamos refatorando, é um pouco menos comum nós mudarmos a assinatura dos métodos, mas pode acontecer. De novo, a ideia de refatorar é manter o comportamento, fazer com que as coisas funcionem como funcionavam antes, sem mudar.

[06:00] Não é uma otimização de performance, nada disso, só que o design do código, a forma como ele é implementado, vamos melhorar um pouco. Vamos fazer algumas alterações para tornar isso mais legível, mais fácil de testar.

[06:14] Então, nosso caso agora, a mesma classe crawler, quando eu tenho um objeto dela, eu posso fazer várias buscas, eu posso realizar várias buscas usando a mesma classe, isso é uma outra vantagem. Com essa implementação, vários dos nossos testes vão quebrar, então, no próximo vídeo, eu volto para falarmos um pouco mais de refatoração e consertar esses testes.

@@04
Para saber mais: Correções

Como foi comentado no vídeo anterior, o projeto utilizado no treinamento é um código real e relativamente antigo. Alguns testes desse projeto passaram a falhar e o Jean já resolveu o problema antes mesmo de eu saber que o problema existia. Então se você também se deparar com os testes falhando, pode conferir a sugestão dele aqui:

https://cursos.alura.com.br/forum/topico-falha-nos-testes-227160

@@05
Propósito

Entendemos neste vídeo o que é refatoração e aprendemos qual o seu propósito.
Para que realizamos refatorações?

Para deixar o código mais rápido
 
Alternativa correta
Para deixar o código mais legível
 
Alternativa correta! Nós sempre devemos nos preocupar em deixar o código mais legível, mais facilmente testável, manutenível, etc. Dessa forma nosso software evolui de forma mais saudável e fica mais fácil de manter.
Alternativa correta
Para deixar o código mais seguro

@@06
Importância de testes

[00:00] E aí, pessoal? Bem-vindos de volta. Como nós modificamos a assinatura dos nossos métodos, precisaremos alterar o código que usa esses métodos, o nosso construtor, a nossa classe. E tem muitos lugares, então esse vídeo será um pouco demorado. Se você quiser fazer sozinho, pode pausar esse vídeo, faça tudo e depois volte, assista acelerado, para garantir que fizemos igual. Mas vamos lá, vou começar por essa aqui.
[00:23] Primeiro eu terei que mudar o nome deste teste. Este teste tenta instanciar um crawler com um domínio que contém o HTTP antes, e isso tem que dar erro. Só que isso agora dará erro na hora de chamar o método get result. Será testTryingToGetResultsWithHttpOnGoogleDomainMustFail().

[00:47] Dá para melhorar esse nome, mas não vamos nos preocupar tanto com isso por agora. Vou tirar a parte new SearchTerm(search Term '') e $domain. Esse será um =new Crawler e esse crawler, quando eu fizer o $crawler->getResults() desse (new SearchTerm(searchTerm: ' ') vazio, passando esse $domain);, isso tem que lançar uma exceção, por isso tem o expectException.

[01:07] No debaixo, com o domínio inválido, então esse também será aquele mesmo cenário, onde o erro passará a acontecer no método getResults, então vamos nessa, $crawler->GetResults(new SearchTerm(searchTerm: ' ') vazio também, de um googleDomain: 'invalid-domain').

[01:32] De novo, isso deve lançar uma exceção. Vamos ver qual é o problema aqui, esse getResults, ele está faltando o country code. Então, aqui tem um detalhe que deixamos passar.

[01:55] Aqui, o countryCode era opcional, ele podia ser vazio. Então precisamos também fazer isso aqui. Só estou refazendo todo o trabalho que eu tinha feito antes. Aqui, o string $countryCode = ' ' pode ser vazio aqui também.

[02:06] Corrigido esse problema, agora não temos mais erro aqui. Vamos continuar. Eu estou criando alguns mocks, eu espero que isso gere uma exceção, eu estou criando uma StreamInterface para ser a resposta.

[02:22] Tenho aqui um proxy, que também será um mock. Isso aqui, moleza, só vou passar esse mock para o nosso getResults.

[02:33] Teoricamente essa classe de teste já está pronta para ser executada. Vamos para o próximo, que é esse, e este dará um trabalho legal também.

[02:38] Vamos nessa, esse ($searchTerm) eu não preciso aqui, eu preciso dele no getResults($searchTerm).

[02:45] Deixa eu ver qual é o problema, Google domain, está faltando, então também posso ter esse Google domain vazio. Posso ter esse Google domain com o padrão, que é string $googleDomain = 'google.com'. Eu poderia dar aquele "Ctrl + Z" de novo, para conferir, mas eu me lembro que o padrão era google.com.

[03:06] Então vamos lá, agora ele não precisa mais, menos um erro. Aqui estamos utilizando alguns proxies. Eu acho que este ainda está funcionando, mas eu posso simplesmente remover esse teste, porque neste treinamento não usaremos os proxies. Então eu posso apagar isso ou eu posso tentar corrigir.

[03:23] Vamos tentar corrigir e ver se ele vai funcionar. Vou usar esse getResults($searchTerm), que pelo menos ele já está configurado para pular caso o proxy esteja fora, então não temos esse problema.

[03:34] Esse debaixo estamos sem implementação, então este código nem será atualizado, nem precisa ser atualizado. Ele nem será executado, no caso. Ok, esse já foi.

[03:46] Agora vamos no personalizado. De novo esse searchTerm vai para o getResults, getResults($serchTerm). No debaixo, a mesma coisa o searchTerm vai para o getResults e aqui vamos passar o 'google.ab' como Google domain. Perfeito, isso é um domínio válido.

[04:11] Só que esse country suffix não existe, então isso terá um problema. A princípio está tudo certo, vamos executar os nossos testes, garantir que eu não fiz nenhuma besteira e que, pelo menos o que já passava antes, continua passando, não temos nenhum erro novo, nem nada do tipo.

[04:26] Enquanto isso vai executando, perfeito, tudo continua passando. Eu queria levantar o ponto que é a importância da escrita de testes quando fazemos a refatoração, porque quando trabalhamos com refatorações, quando temos um código que funciona e modificamos ele precisamos de alguma forma garantir que ele continue funcionando.

[04:49] Por isso eu peguei esse projeto, que é antigo, mas tem alguns testes. É interessante também vermos um cenário real, que infelizmente temos testes desatualizados, que marcamos para resolver depois, temos testes incompletos, que não conseguimos resolver na hora. Isso é um cenário real, esse é um código que realmente esteve em produção, que ele foi utilizado em cenários reais, em empresas reais.

[05:10] Então aqui estamos refatorando, e vamos refatorar muito mais, um projeto real. Sem testes, essa tarefa seria muito mais difícil. Repare que quando eu fui corrigindo os testes, eu percebi enganos que eu tinha cometido, como não deixar os parâmetros com os valores padrão, como eram antes, enfim, então esse vídeo foi para ressaltar a importância de testes quando fazemos refatorações.

@@07
Por que testes?

Neste vídeo nós vimos que testes automatizados ajudam (e muito) no processo de refatoração.
Em que aspecto os testes nos ajudam no processo de refatoração?

Eles nos ajudam a pensar melhor nos detalhes intrínsecos do código para refatorar.
 
Alternativa correta
Na verdade é apenas um capricho. Testes não são úteis neste cenário.
 
Alternativa correta
Eles nos dão uma maior confiança de que nenhum bug foi introduzido.
 
Alternativa correta! Com os testes passando antes e depois da refatoração, temos uma confiança maior de que o sistema continua funcionando conforme esperado.

@@08
Pequenos passos

[00:00] E aí, pessoal? Bem-vindos de volta. No vídeo anterior nós entendemos a importância dos testes, porque no primeiro vídeo, onde fizemos a nossa primeira refatoração, nós fizemos algo que não é comum, que não é uma boa prática quando estamos refatorando: nós fizemos muitas coisas ao mesmo tempo, mudamos bastante coisa.
[00:18] O que acontece? Uma boa prática, quando vamos refatorar um código, é fazer isso em pequenas etapas, dar pequenos passos, fazer pequenas refatorações. Aqui, nós poderíamos ter quebrado aquela refatoração em mais de uma, mas eu queria mostrar o que é uma refatoração, que é mudar o código, mexer no código, sem alterar o funcionamento, fazendo com que tudo continue funcionando.

[00:41] Agora vamos fazer uma refatoração um pouco mais correta. Não vamos alterar nenhuma assinatura de método, vamos fazer uma modificação pequena e nenhuma funcionalidade será alterada, eu não precisarei mexer em nenhum teste, por exemplo. O que podemos mudar?

[00:57] Esse nosso construtor aqui, ele já foi bastante melhorado, ele já está bem menor, só que tem um detalhe: essa sintaxe pode ser melhorada. Existe um operador, que não é tão novo assim, mas que é o operador de null coalesce, é um operador de coalescência nula. Tem um vídeo aqui, na Alura+, falando sobre ele.

[01:16] O que eu vou fazer é: eu vou pegar esse proxy, caso ele exista. Agora, se ele for nulo, então eu vou criar um novo sem proxy, ou seja, um proxy que é um objeto falso, que não tem proxy, na verdade, porque esse código, que busca no Google, às vezes o Google bloqueia o seu crawler, bloquei o seu robô, então precisamos de um proxy.

[01:38] Nós temos esse NoProxy(), ou seja, fazer requisição sem proxy nenhum e nós temos duas outras classes de proxies, uma que faz requisições em vários proxies em comum, por isso se chama common proxy, e outra que ia em um site chamado key proxy, por isso o nome da classe é key proxy.

[01:54] Mas, se não passarmos nenhum proxy aqui, nós vamos utilizar o NoProxy. Então agora demos uma melhorada nesta linha, deixamos ela um pouco mais sucinta. Repare que eu consigo bater o olho e ver: eu estou pegando o proxy se existir, caso contrário, eu crio um novo.

[02:08] A legibilidade não foi comprometida, eu a melhorei um pouco, e o funcionamento continua idêntico. Eu posso vir aqui, rodar os meus testes de novo, que o resultado é o mesmo: eu terei alguns testes incompletos, alguns sendo pulados, mas nenhum teste vai falhar, por causa dessa implementação.

[02:24] Então a ideia de uma refatoração é essa: pequenas modificações no código, que não alteram a interface de um método, ou seja, a assinatura do método, não alteram a pay pública dele, que não altera o comportamento, não necessariamente faz alguma otimização de performance, nem nada disso.

[02:41] Então uma refatoração real é esse tipo de coisa aqui, nós substituímos um algoritmo por outro, por exemplo, que é inclusive o nome de uma refatoração, mas enfim, essa é a ideia por trás realmente de refatorar: não alterar o comportamento, mas deixar o código um pouco mais legível.

@@09
Mexer em tudo

Neste vídeo foi falado que é interessante fazermos pequenas modificações no código por vez quando estivermos refatorando.
Por que não é ideal realizar grandes alterações de uma só vez?

Selecione uma alternativa

Porque quanto maior a modificação, maiores as chances de introduzir bugs
 
Alternativa correta! Se mexemos em uma grande quantidade de código de uma só vez, temos maiores chances de introduzir bugs do que se mexemos em pequenas partes do código.
Alternativa correta
Porque assim temos mais modificações em menos tempo, mostrando produtividade
 
Alternativa correta
Fazer grandes alterações ou ir em pequenos passos dá no mesmo no final das contas

@@10
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@11
O que aprendemos?

Nesta aula:
Conhecemos um projeto real que vamos refatorar
Aprendemos o que é refatoração
Vimos que testes são um passo muito importante na hora de refatorar
Entendemos que ao refatorar, devemos dar pequenos passos