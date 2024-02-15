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

#### 12/02/2024

@02-Métodos

@@01
Projeto da aula anterior

Caso queira, você pode baixar aqui o projeto do curso no ponto em que paramos na aula anterior.

https://github.com/alura-cursos/google-crawler/archive/refs/tags/aula-1.zip

@@02
Extrair método

[00:00] E aí, pessoal? Bem-vindos de volta a mais um capítulo deste treinamento, onde vamos começar a brincar com refatorações usando PHP como linguagem. Como eu falei, esse é um projeto real e projetos reais têm problemas de código. Vamos dar uma olhada aqui, em algum problema que identificamos, e como podemos melhorar pelo menos um pouco. Então esse método getResults, nós já vemos de cara que ele está meio grande.
[00:23] Além de ele estar grande, ele faz muitas coisas. Primeiro ele verifica se esse googleDomain é válido, é algo que faz sentido, que está dentro do padrão. Ou seja, começa com google.com.

[00:36] A partir disso, ele monta a URL, chama o método que faz isso, ele pega a resposta, usando o nosso proxy. A partir desta resposta, ele busca a lista de resultados, monta uma lista de resultados e, a partir disso, ele monta aquela googleResultList, ele atribui e insere todos os resultados, fazendo o parse nessa nossa lista de resultados.

[00:58] Então, o que acontece? Temos bastante coisa acontecendo aqui e podemos começar a quebrar cada uma dessas partes. Uma refatoração muito comum é pegar um pedaço de código, que faz alguma coisa e que é normalmente repetido em outros lugares, e separar ele em um método específico.

[01:16] No nosso caso, não temos essa repetição de código, mas algo assim é muito comum, termos a repetição, dois métodos que chamam duas, três linhas em comum. É muito comum pegarmos essas duas, três linhas, e extrairmos para um método específico. No nosso caso, eu vou extrair isso.

[01:34] Esse pedaço de código $googleResultlist. Por quê? Ele tenta buscar os resultados do Google e isso pode mudar frequentemente. Se o Google mudar um CSS deles, eu preciso alterar esse método. Então eu vou deixar essa parte em um lugar separado, para não mexer nesse método grande. Além disso, ele faz uma verificação, pode lançar uma exceção. Então sendo assim, isso não é um algoritmo que eu julgo digno de virar um método próprio.

[01:58] Dessa forma vamos extrair esse método. Se você está utilizando o PhpStorm, você pode fazer o "Ctrl + Alt + M", ou no Mac "Command + Option + M". Aqui ele já vai realizar essa refatoração de extrair método. Só que eu não vou utilizar a IDE aqui, para fazermos de forma manual. Eu vou pegar, copiar esse código, e fazer com que $googleResultList = seja igual a um novo método, que é a =$this->$googleCreateList).

[02:28] Eu sei que esse método vai precisar desse ($domCrawler). Com esse domCrawler em mãos, podemos criar esse novo método.

[02:40] E aqui eu vou chamar de private function createGoogleResultList(). Ele espera um (DomCrawler $domCrawler). Ele executa esse código e, no final, retorna esse return $googleResultList. Qual é o retorno desse filterXPath? Isso é um crawler. Então vamos devolver esse crawler, só que é um DomCrawler que ele retorna, porque eu estou utilizando aquele alias.

[03:08] Só para você entender do que eu estou falando, de alias, a minha classe se chama Crawler e temos uma classe que se chama também Crawler, de um componente externo, então demos um apelido para ela, de DomCrawler.

[03:20] Por isso ela está retornando esse tipo DomCrawler. Então ela recebe um crawler, busca alguma coisa e retornar um novo crawler só desses elementos.

[03:30] Caso você não esteja entendendo muito bem essa parte de fazer web scraping, tem um treinamento de web scraping aqui na Alura, talvez isso fique mais claro. Mas aqui, tudo o que estamos fazendo é pegando esses elementos, filtrando eles e devolvendo um novo buscador de elementos, mas que já começa nesses elementos que nós buscamos. Basicamente é isso.

[03:50] Agora o nosso método ficou um pouco menor, um pouco mais simples, ele agora cabe em uma tela só, por exemplo. Claro que a minha tela está muito ampliada, se fosse uma resolução normal ele já caberia antes, mas isso é um bom sinal, ele diminuiu de tamanho e fica um pouco mais fácil de ler.

[04:05] Mas, obviamente, tem muito mais coisa para melhorar. Esse método ainda está grande, ainda tem bastante coisa nessa classe em si que dá para melhorar. Só que, de novo, lembra daquele detalhe de pequenas etapas quando estamos refatorando? Eu fiz uma refatoração. Foi uma refatoração pequena, foi um único processo que eu fiz, mas eu posso vir aqui e realizar os meus testes, phpunit, para garantir que tudo continue passando.

[04:30] Talvez você esteja se perguntando: “Vinícius, normalmente os testes são rápidos, por que esses testes estão demorando tanto?” Porque eu tenho muitos testes de integração aqui, que vão ao Google, fazem a busca mesmo, fazem uma requisição HTTP para fazer esse parse de resultados, por isso está demorando um tempo.

[04:45] Mas, entendido esse processo, vamos avançar nesse nosso estudo de extração de métodos, de catálogos, de refatorações. O que mais podemos mexer nesse código para deixar esse ele melhor?

@@03
Namespace alias

No código deste projeto nós temos um alias da classe Crawler de um componente externo para DomCrawler.
Qual a utilidade deste alias no código do projeto?

Não há motivo real para ter isso ter sido feito
 
Alternativa correta
Renomear uma classe para seu nome fazer mais sentido
 
Alternativa errada! Não é esse o motivo de termos um alias no projeto.
Alternativa correta
Evitar conflito com a classe Crawler do próprio projeto
 
Alternativa correta! O projeto possui uma classe Crawler e um componente externo que ele usa possui uma classe com mesmo nome. Através de Namespace Alias nós conseguimos usar ambas sem conflitos.

@@04
Internalizar método

[00:00] E aí, pessoal? Bem-vindos de volta. No último vídeo nós refatoramos o método getResults para que no momento de criar essa lista de resultados, isso fosse feito por um outro método, um método separado. Você já deve ter percebido que essa classe tem vários métodos separados para fazer essas pequenas tarefas. Vamos dar uma olhada na parte que faz o parse de um elemento do dom para transformar ele em um resultado realmente.
[00:24] Nesse método aqui, nós fazemos isso, nós parseamos, e tem algumas tarefas que ele faz. Primeiro ele pega o link desse resultado do Google, verifica se é válido. Depois do link, ele pega a descrição desse elemento e verifica se é válido. Antes de continuar, antes de realmente retornar esse resultado do Google, verificamos se é uma sugestão de imagem, porque se for, não vamos colocar isso nos nossos resultados.

[00:48] Porque essa biblioteca aqui, esse código, esse crawler, ele só se interessa nos sites e não nas sugestões de imagens. Se for uma sugestão de imagem, não vamos adicionar esse resultado. Mas repare que aqui nós já estávamos em um método, entramos em outro método e vamos para outro método, que não tem um código muito complexo, ele não tem muito algoritmo para ser realizado.

[01:12] Então assim como temos uma refatoração de extrair método, que foi o que fizemos no último vídeo, existe uma refatoração que é incorporar método, que é exatamente o contrário, é pegar um método e trazer ele para dentro do método que chama, porque ele não é utilizado em outros lugares, ele não tem uma complexidade muito grande, então não é justificado ele estar separado.

[01:34] O que vamos fazer? Em isImageSuggestion, nós já sabemos que ele é chamado em um lugar só. Por quê? Aqui, no PHP Storm, se eu dou um "Ctrl" e clique, ele já vai direto para o local, ou seja, só aqui ele é chamado. Então o que eu vou fazer é copiar o código dele, que eu sei que é isso, se isso é maior que 0.

[01:52] Vamos lá, fazer por pequenas etapas primeiro, que é aquele processo. Ao invés de copiar esse código e já internalizar, eu vou refatorar esse código aqui. Eu vou fazer com que ele esteja em uma linha só, para que eu já retorne se o count() > 0. Pronto, já retornou, me livrei de uma linha. Primeira etapa.

[02:12] Aqui eu precisaria rodar os testes para garantir que tudo continua funcionando, mas eu vou poupar esse tempo, já que temos muitos testes de integração. Vou pegar essa linha return $resultCrawler, vou copiar, remover esse método e trazer para cá. Vou no if ($this->isImageSuggestion e colar.

[02:30] A partir do resultCrawler, ele está fazendo um filtro, verificando se tem uma imagem lá dentro. Se tiver alguma imagem, ou seja, se a contagem de imagens for maior que 0, eu sei que isso é uma sugestão de imagem, então não vou entrar neste detalhe. Repare que o código está simples, eu não prejudique a leitura, porque aqui está bem claro.

[02:48] Além de ter uma descrição na exceção que eu estou lançando, a própria linha de código é bastante clara: se tiver alguma imagem nesse meu resultado, nesse elemento de resultado, significa que eu não quero adicionar esse resultado na minha lista. Então nós fizemos o que é conhecido como internalizar método, que é o contrário de extrair o método, em inglês é method inline ou inline method.

[03:13] Então pegamos um pedaço de código, que só é utilizado em um lugar e não é muito útil ter ele em um lugar diferente, ou seja, o algoritmo não é complexo, ter um nome por ter um método assim não ajuda tanto, porque às vezes, se temos um algoritmo complexo, é interessante fazer aquela refatoração de extrair método, só para ter um nome ali.

[03:35] Eu sei que esse pedaço de código faz isso, porque tem um nome, o método tem um nome, já o bloco de código não. Sendo assim, se temos um método que só tem uma linha de código ou tem um algoritmo muito simples, e o nome dele não ajuda tanto na legibilidade, como era o caso aqui, podemos fazer esse method inline ou inline method.

[03:53] Claro que isso é relativo, às vezes refatorações podem acabar causando uma maior complexidade no código, na leitura, então eu vou deixar para você avaliar: você preferia com o método isImageSuggestion ou você prefere agora, sem um método extra, para precisarmos ficar dando "Ctrl" e clique para ver o que ele faz?

[04:12] Você vai decidir se mantém essa refatoração ou não. Mas aqui nós conhecemos um novo tipo de refatoração, que é fazer inline de um método.

@@05
Extrair variável

[00:00] E aí, pessoal? Bem-vindos de volta. No último vídeo nós vimos que normalmente técnicas de refatoração tem sua contraparte. Eu posso extrair um método, caso isso facilite a leitura, ou eu posso incluir um método, incorporar um método, fazer um inline de um método, se isso não for dificultar a leitura, se isso não for transformar o nosso código em uma confusão.
[00:22] Só que nem só de métodos vive a programação, então o que vamos fazer agora é começar a trabalhar com variáveis. O que acontece? Nós incorporamos esse método if ($resultCrawler->filterXPath, que é uma única linha, e esse método só existia para dar um nome a essa linha.

[00:37] Então, o que podemos fazer? Ao invés de ter um método, que está em outro pedaço de código, nós precisaríamos ir lá, no caso de uma IDE, fazer um "Ctrl + click" e clique para navegar para outro lugar, eu posso fazer isso: eu vou extrair isso para uma variável ao invés de um método, que o código ficará aqui mesmo. Eu vou copiar e criar uma $isImageSuggestion e colei aqui.

[00:59] Agora, nesse if, ao invés de ter esse código, eu posso utilizar um nome. Repare que eu estou verificando se é uma sugestão de imagem, mas eu não preciso ir em um outro método para saber que é isso o que essa linha de código faz.

[01:11] Então, essa ideia de extrair uma variável, é simplesmente pegar um pedaço de código, uma linha um pouco mais complexa, e dar um nome para ela. Só que para eu dar um nome para essa linha, eu não preciso de um método, eu simplesmente extraio uma variável. Agora que já estamos começando a ficar familiarizados com essas técnicas, eu vou dar alguns "Ctrl + Z" aqui para começarmos a utilizar a IDE ao nosso favor.

[01:35] Eu estou utilizando o PHP Storm, mas outras IDEs tem outros atalhos, tem outras formas de você refatorar. Eu vou selecionar esse mesmo pedaço de código e eu tenho duas opções: eu posso vir com o botão direito, em refatorar e introduzir variável, ou seja, eu vou transformar isso em uma variável, eu posso fazer isso aqui.

[01:55] Ou, "Ctrl + Z" de novo, eu posso fazer "Ctrl + Alt + V", ou "Command + Option + V" no Mac. O que isso vai fazer? Vai introduzir uma nova variável, pega esse código e extrai para uma variável. Eu vou chamar ela de isImageSuggestion, simples assim.

[02:12] Dessa forma, de novo, eu pego um pedaço de código, que é uma linha, um algoritmo simples, dou um nome para ele, sem precisar criar um novo método. Essa é a funcionalidade de extrair uma variável ou introduzir uma variável no nosso código.

@@06
Nova variável

Neste vídeo a refatoração foi bastante simples. Criamos uma nova variável com a expressão que estava sendo avaliada pelo if
Por que nós criamos uma nova variável neste vídeo?

Para que o valor analisado pelo if seja alterado pelo usuário
 
Alternativa correta
Para que aquela expressão tivesse um nome facilmente legível
 
Alternativa correta! É muito mais fácil nós lermos um único nome do que uma expressão completa. Esse é o propósito dessa refatoração que fizemos.
Alternativa correta
Para otimizar a performance do if

@@07
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@08
O que aprendemos?

Nesta aula:
Começamos a refatorar um projeto do mundo real
Aprendemos a extrair métodos
Vimos como e quando podemos internalizar códigos de métodos
Entendemos que uma simples variável po

#### 13/02/2024

@03-Variáveis

@@01
Projeto da aula anterior

Caso queira, você pode baixar aqui o projeto do curso no ponto em que paramos na aula anterior.

https://github.com/alura-cursos/google-crawler/archive/refs/tags/aula-2.zip

@@02
Incorporar variável

[00:00] E aí, pessoal? Bem-vindos de volta. Como nós começamos a falar um pouco de variáveis no vídeo anterior, no final do capítulo anterior, vamos continuar nesse assunto. Vamos ver quando usamos um método, quando usamos uma variável, enfim. Eu tenho aqui uma Google URL, ou uma URL do Google, que está sendo montada a partir de um método.
[00:19] Repare que eu preciso passar três parâmetros para esse método, todos esses parâmetros que eu já tenho acessíveis aqui, e quando eu vou lá dentro, esse $domain = $googleDomain nós já sabemos que não precisa, eu já poderia trazer direto para cá, então quebra uma linha.

[00:35] Este return $url, se usássemos lá direto, também não precisaria. Então, na prática, temos a inicialização de uma variável e um if. De novo, esse método não nos traz tanta vantagem, então podemos, mais uma vez, fazer aquela ideia de incorporar um método. Nós vamos substituir um método por uma variável. Vamos lá, vamos pegar esse código, deixa eu dar um "Ctrl + click" no getGoogleUrl, vou trazer ele para cá. Só que eu vou renomear, ao invés de $url, eu vou chamar de $googleUrl.

[01:13] Então eu peguei aquele método que não fazia muita coisa, aquele método que não tinha uma lógica muito complicada, e trouxe para cá. E o que eu estou fazendo aqui? Eu estou substituindo um método por uma variável, simples assim. Porque, dessa forma, eu deixo o código mais claro, direto aqui no método que utiliza essa URL. Aquele nosso método getGoogleUrl, ele não é mais usado em lugar nenhum.

[01:36] Então era um método bastante simples, que precisava de muitos parâmetros para ser chamado em um lugar só. Nesse caso, podemos facilmente remover esse método e diminuir o tamanho da nossa classe como um todo. De novo, esse método getResults ainda pode ser muito melhorado, mas ele está com uma cara um pouco mais interessante, onde faz sentido, onde temos exceções sendo lançadas e, nós temos um método a parte.

[02:02] Nós estamos delegando parte da responsabilidade para outras classes, mas onde faz sentido, onde podemos, nós estamos utilizando direto aqui, estamos incorporando. Então estamos diminuindo o número total de métodos da nossa classe, sem aumentar muito o tamanho de um método só. Nessa conversa de quando utilizar a variável, quando utilizar método, podemos acabar entrando muito em opiniões.

[02:26] No próximo vídeo nós vamos bater um papo sobre quando faz sentido fazer isso, e também o que eu fiz no último vídeo, que foi incorporar essa variável. Então quando faz sentido incorporar uma variável ou quando faz sentido fazer aquilo que eu fiz no primeiro vídeo, de extrair um método separado. No próximo vídeo vamos falar isso, quando usar método e quando usar variável.

@@03
Variável vs Método

[00:00] E aí, pessoal? Bem-vindos de volta. No último vídeo fizemos uma refatoração, se você se lembrar bem, essa refatoração que nós fizemos, nós já conhecemos como inline method, ou incorporar método. Já no vídeo anterior, nós tínhamos aplicado uma refatoração que se chama extrair variável.
[00:21] Nós temos um algoritmo e extraímos para uma variável. Só que antes disso, nós tínhamos feito um inline de um método também, então às vezes precisamos extrair uma parte de código, às vezes precisamos fazer um inline de uma parte de código. Então quando fazemos isso?

[00:36] E nesse cenário, nós acabamos trocando a chamada de um método por uma variável. Quando faz sentido ter um, quando faz sentido ter outro? No nosso caso, nós temos uma classe relativamente grande, então ela pode diminuir, inclusive talvez falemos mais sobre isso, sobre diminuir essa classe. Ela tem muitos métodos, repare quantos métodos ela tem.

[00:57] Então, nesse cenário, talvez diminuir o número de métodos seja uma coisa boa, porque temos menos indireções, que é o que conhecemos. Uma indireção é quando eu chamo um método, esse método chama outro e esse método chama outro. Então eu tenho várias indireções, eu tenho várias chamadas de outros métodos para realizar o trabalho.

[01:19] Então quando eu tenho uma classe um pouco complexa como essa, se eu posso diminuir o número de indireções, isso pode ser vantajoso, desde que o algoritmo seja simples, que esse algoritmo faça parte dessa classe mesmo e não seja uma coisa que eu deva extrair para outra classe, por exemplo, enfim.

[01:37] Nesse cenário, onde simplesmente estávamos montando a URL do Google, repare que isso é uma coisa que não vai mudar. O Google não vai mudar a forma de montar uma URL, a rota do Google, de search, não vai mudar sempre. Então isso eu não preciso botar para o método separado, para um local separado, e eu não tenho um algoritmo complexo, eu só tenho um if para garantir.

[01:59] Se eu tiver informado o $countryCode, se eu tenho o código do país, eu vou adicionar ele como um outro parâmetro na nossa URL, e só, é simples, é um código simples. Agora, nesse cenário de criar o resultado do Google, nós tínhamos uma verificação que lançava uma exceção, talvez isso possa ser extraído, porque aqui já temos um código que pode ser alterado com mais facilidade, tem esse detalhe.

[02:28] Isso aqui, inclusive, já mudou muito, muitas vezes. Essa classe, que representa o resultado do Google, muda frequentemente. Esse é um código que será bastante alterado, então é bom isolar ele, é um código que lança uma exceção, dependendo de determinados casos. Talvez, às vezes, queremos alterar a mensagem dessa exceção, inclusive eu acho que tem até um erro aqui de escrita.

[02:50] É "parsable" e não "parseable". Repare que é um código que vamos alterar mais vezes, então é interessante isolar ele em um lugar, em um local específico. Eu ainda acho que esse pedaço de código faz sentido estar dentro dessa classe de crawler, porque ele manipula diretamente o dom, ele manipula diretamente o HTML do Google. Então ele faz parte, é responsabilidade de um crawler fazer isso.

[03:16] Então eu acho que esse método está na classe certa, só que eu acho que ele deve ser um método separado por causa desses detalhes. É um algoritmo que eu posso ter modificações sempre, eu posso, às vezes, querer mudar o tipo dessa exceção, criar uma exceção mais específica. É um algoritmo que pode mudar mais vezes, então eu extraí ele.

[03:36] Agora, nesse cenário de isImageResult, eu sei que um detalhe, uma linha de código que não precisa de verificação nenhuma, não estou fazendo nada além de simplesmente pegar esse resultado e ver se existe.

[03:49] Então, claro, se a partir de algum momento a forma de identificarmos se é uma imagem ou não começar a mudar, extrair isso para um método pode fazer sentido. Repare que nem sempre as coisas são tão objetivas, e para podermos decidir que tipo de refatoração implementar, nós precisamos examinar qual é o problema.

[04:09] Não vamos simplesmente olhar para a nossa classe ou para um método e pensar: “poxa, vou melhorar isso aqui, vou sair modificando sem pensar muito bem”. Nós precisamos identificar problemas e, a partir desses problemas, nós sabemos qual técnica de refatoração implementar. Nós vamos falar bem mais sobre um catálogo de técnicas de refatoração no final do treinamento.

[04:30] Mas, por enquanto, é importante entendermos que estamos conhecendo essas técnicas, incorporar método ou extrair método, incorporar, extrair uma variável, ou incorporar o método como uma variável. Nós estamos começando a conhecer as técnicas e no final do treinamento, vamos entender como identificar um problema e, a partir desse problema, saber usar a técnica adequada.

[04:53] Mas já falei bastante, vamos voltar a refatorar, vamos voltar a botar a mão em código. Por exemplo, será que essa variável $googleResult é necessária? Será que eu preciso dela? Quando faz sentido ter uma variável, modificar uma variável? Quando criar uma outra? Enfim, vamos continuar falando de variáveis no próximo vídeo.

@@04
Quando usar cada?

Tivemos uma discussão interessante sobre quando extrair um método ou quando apenas introduzir uma nova variável.
Em que cenário é mais vantajoso nós extrairmos um método?

Quando a expressão que queremos nomear é simples
 
Alternativa correta
Quando o algoritmo que queremos nomear é complexo
 
Alternativa correta! Às vezes nós queremos dar nome a um pedaço de código que executa um algoritmo um pouco mais complexo. Talvez ele lance uma exceção, talvez seja suscetível a mudanças. Nesses cenários, extrair um método pode ser mais vantajoso.
Alternativa correta
Sempre que queremos dar nome a um pedaço de código
 
Alternativa errada! Às vezes simplesmente introduzir uma variável já é o suficiente.

@@05
Quebrar variáveis temporárias

[00:00] E aí, pessoal? Bem-vindos de volta. Como eu comentei no último vídeo, será que essa variável é necessária? Será que ela está ajudando em alguma coisa? Ela traz algum valor, para essa variável, logo depois retornar?
[00:12] Às vezes, e é bastante comum, temos uma variável só para usar ela uma única vez na linha seguinte, para ter algum nome, mas nem sempre isso é necessário. Por exemplo, aqui eu tenho uma variável temporária, ela não é usada depois, ela é realmente temporária e o direta retorna ela.

[00:27] Então, nesses cenários onde eu tenho uma variável só para usar na linha seguinte e depois não utilizar mais, ela é temporária no sentido de que eu só vou utilizar ela uma vez, para um detalhe específico, e o código que foi utilizado para gerar o valor dela não é complexo, podemos fazer o que é conhecido como inline de variáveis temporárias ou inline temporary variables.

[00:49] Vamos incorporar direto no código o que seria essa variável, vamos direto utilizar esses algoritmos. Às vezes nós temos, por exemplo, aqui na linha 134, nós extraímos uma variável temporária.

[01:02] Então essa é a contraparte dessa refatoração. Às vezes, se isso fosse um código um pouco mais simples, que não precisasse do nome, eu poderia, ao invés de ter a variável, simplesmente utilizar esse código diretamente no if. Repare que a maioria das técnicas de refatoração possui uma contraparte.

[01:20] Eu posso extrair algo, posso fazer um inline de algo, posso criar uma variável temporária ou posso me livrar de uma variável temporária, então a maioria das técnicas de refatoração possui contraparte. Mas como essa foi bem simples, vamos procurar um outro lugar para refatorarmos, vamos ver esse tal de proxy.

[01:38] Vamos ver no nosso NoProxy, o que mais estamos utilizando, e esse método está me incomodando bastante. O que esse método faz? Ele é bem chato de entender, porque as funções são complicadas.

[01:49] Ele pega uma URL e a separa em partes de URL. Repare que até precisamos de um comentário aqui. Talvez link não seja o melhor nome, talvez chamássemos isso de partes da URL. Então vamos lá, vamos renomear essa variável, $urlParts ou alguma coisa do tipo, podemos até achar algum nome melhor talvez. Só que o que acontece.

[02:13] Eu estou utilizando essas partes de URL, ou seja, eu estou separando as partes de URL, não preciso mais do comentário, e dessas partes de URL eu estou pegando a query string e dessa query string eu estou fazendo um outro parse. Por que, o que acontece? Deixa eu abrir o terminal.

[02:29] O resultado do Google, ele retorna mais ou menos assim: "google.com/algumacoisa", alguma coisa que eu não me lembro o que é, e temos na query string, o "q=" e temos a URL do nosso resultado. Por exemplo, se o resultado for o "dias.dev", imagine que é o meu blog, que buscamos pelo meu blog.

[02:49] Essa é a URL do resultado do Google, porque o resultado do Google não tem o link do site direto, ele tem um site do próprio Google, que ele analisa algumas métricas, antes de retornar para os sites corretos. Então o que eu estou fazendo aqui é com essa primeira linha, eu separo essa URL em domínio, em caminho ou path, e na nossa query string.

[03:17] Então ele vai pegar esses detalhes de query string. Pode ter esse "q=" que tem a URL, e pode ter outras coisas, não sei, "temp=algumacoisa", "etc=outra", então ele pode ter vários parâmetros aqui. A partir desses vários parâmetros que eu posso pegar dessa query, eu estou fazendo o parse string. O que esse parse string faz?

[03:39] A partir de uma URL, que tenha esse formato que é um valor igual a outra coisa separado por "&", então valor outra coisa, "&", uma valor, um parâmetro, uma chave, valor, "&", chave, valor, "&", ele pega isso e separa em um array. Então essas não serão mais as partes da URL. Eu tenho uma variável e depois eu estou sobrescrevendo ela aqui.

[04:06] Repare que está difícil, está um código complexo. Deixa eu fechar o terminal. Então isso aqui agora irá conter o nosso link. Como podemos chamar essa variável? Nós vamos criar uma outra variável aqui. Vou apertar aqui, no PHP Storm, utilizando o Mac eu vou apertar o "Ctrl + G", que ele irá selecionar essa duas $urlParts, e eu vou dar um novo nome, porque eu não estou gostando disso.

[04:30] Eu vou chamar de $queryStringParams, porque eu peguei as partes da minha URL do Google. Eu pego essas partes da URL e faço o parse para pegar as query strings, os parâmetros da query string. Então não preciso mais desse comentário também.

[04:52] Agora eu pego a URL a partir do parâmetro ['q'] dessa query string. Então a minha query string, como eu mostrei na URL do Google, ela tem um parâmetro "q" e lá tem a URL. Eu estou fazendo a verificação, então aqui eu não preciso desse comentário também, porque essa verificação está mais clara.

[Aula3_video3_imagem3]

[05:08] Então o que eu fiz aqui? Eu estou introduzindo uma nova variável para não utilizar somente variáveis temporárias, que era aquele link. Só que, antes disso, eu renomeei uma variável, que também é algo que não faz parte de um catálogo de refatorações, mas é uma técnica muito válida, mudar o nome de uma variável para que ela faça mais sentido, para que ela tenha um significado um pouco mais correto.

[05:31] Então aqui, como eu mudei bastante coisa, vou rodar junto com vocês o nosso teste, os nossos testes. Mas, enquanto isso, vamos recapitular o que eu fiz. Eu primeiro renomeei a variável, para invés de ser link, o que não quer dizer muita coisa, eu chamei de $urlParts, ou partes da URL.

[05:48] Dessas partes da URL, eu pego a query string e faço um parse de string. Isso é uma função do PHP. Esse parse de string pega aquela chave e valor, separados por "&", e transforma em um array. Então eu terei aqui os parâmetros da query string, os query strings parameters. Eu pego o parâmetro "q" e verifico, eu garanto que é uma URL.

[06:13] Então se eu consigo pegar essa URL, eu retorno ela. Os nossos testes continuam passando. E esse código, que continua complexo, mas porque a utilização dessas funções é complexa, mas agora, esse código que é complexo, está um pouco mais legível. Sem comentários eu consigo bater o olho nesse código e, com um pouco de esforço, ainda consigo entender o que ele faz.

[06:34] Então aqui nós vimos mais de uma técnica. Nós vimos a inclusão de variáveis, para não ficarmos sobrescrevendo a mesma variável, e vimos também renomear variáveis. Nós não estamos colocando qualquer nome em uma variável, mas sim utilizando um nome que faça sentido.

[06:57] Então aqui foram algumas técnicas, por isso eu rodei o teste aqui, junto com você, mas eu quero que você pare, faça os exercícios e reflita sobre o que vimos aqui, e pense se dá para aplicar algum outro tipo de refatoração. Já te adianto que sim, tem outra refatoração que podemos aplicar, mas talvez o código não fique mais limpo.

@@06
Para saber mais: Funções usadas

Neste vídeo nós usamos 2 funções relativamente complexas de entender, então pra garantir que você compreendeu bem o código, vale a pena dar uma conferida na documentação oficial dessas 2 funções.
Aqui estão:

https://php.net/parse_url
https://php.net/parse_str

@@07
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a pr

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@08
O que aprendemos?

Nesta aula:
Aprendemos que nem sempre um método separado é necessário
Conversamos sobre quando usar métodos ou variáveis
Aprendemos que não devemos reutilizar variáveis

#### 14/02/2024

@04-Substituindo Algoritmo

@@01
Projeto da aula anterior

Caso queira, você pode baixar aqui o projeto do curso no ponto em que paramos na aula anterior.

https://github.com/alura-cursos/google-crawler/archive/refs/tags/aula-3.zip

@@02
Remover atribuição a parâmetro

[00:00] E aí, pessoal? Bem-vindos de volta a mais um capítulo deste treinamento onde estamos conhecendo algumas técnicas de refatoração. Claro, primeiro tivemos que entender o que é essa tal de refatoração, e estamos fazendo isso em um projeto real, por isso tem bastante coisa para melhorar e, mesmo no final do curso, terá bastante coisa para melhorar ainda. Mas vamos lá, vamos direto ao ponto.
[00:18] Esse código da última aula está pequeno, mas eu quero apontar um problema que pode acontecer em códigos maiores. Nós recebemos um parâmetro chamado $url e depois sobrescrevemos esse parâmetro com outro valor, nós utilizamos esse mesmo nome para criar uma variável. Isso pode gerar um problema grande, porque aqui, nesse cenário, estamos fazendo essa sobrescrita, estamos modificando esse valor e retornando ele logo depois.

[00:45] Só que se fosse um cenário onde tivéssemos esse código aqui, o que acontece? A partir desse momento, ainda mais em um código grande, que infelizmente é comum no mundo real, nós não sabemos se o valor que recebemos por parâmetro, continua sendo o original.

[01:00] Nós modificamos e isso torna o nosso código mais frágil, porque podemos precisar do parâmetro de novo, nós não temos mais, podemos acabar utilizando esse valor dessa variável achando que é o parâmetro original, mas, na verdade, ele foi modificado. E pior, no nosso cenário, nós recebemos uma URL e estamos substituindo ele por algo que é realmente uma URL.

[01:23] Então, nesse cenário, podemos acabar realizando manipulações que fazem sentido, que vão funcionar, só que o código não será o esperado, talvez tenhamos um cenário do nosso teste passando, mas mesmo assim, o código não estará correto, não funcionará. Essa é uma técnica muito perigosa, a de substituir o valor de um parâmetro.

[01:41] Então o que faremos aqui é simplesmente renomear essa variável. Ao invés de utilizar essa variável, nós daremos um novo nome específico para ela, para não substituirmos o nosso parâmetro. Vamos lá, essa é a URL real do nosso resultado, então eu posso chamar de result URL ou alguma coisa do tipo, então $resultUrl.

[02:03] Ou seja, isso é a URL do resultado e não a URL do Google. Inclusive eu posso vir aqui e modificar esse parâmetro $url também, renomear esse parâmetro para ser $googleUrl, porque eu sei que, opa, aqui nós temos alguns detalhes de outras classes. Deixa eu ver, onde ele está sendo usado. Acho que isso aqui ele está modificando em outras classes também. Então não vou renomear em outras classes, eu vou renomear só aqui.

[02:32] Ele nos avisa que tem um detalhe, porque na interface que nós estamos implementando, o nome desse parâmetro é URL, já aqui mudamos $googleUrl. Então vamos fazer isso? Aqui, na interface, eu vou renomear também, para $googleUrl, vamos lá, "googleUrl". Só que eu não vou utilizar o rename do PHP Storm, senão ele vai fazer aquilo, ele vai querer renomear em todas as classes, então eu vou renomear só aqui, $googleUrl.

[03:02] Depois, caso eu queira utilizar aqueles proxies, eu entro em cada um dos proxies e renomeio também para alterar, até porque esse código, nos proxies, é diferente, então eu precisaria dar uma olhada para ver o que precisa ser modificado. Mas, no nosso cenário, eu já estou bastante satisfeito.

[03:17] Eu sei agora que eu recebo a URL do Google, eu pego as partes dessa URL, faço o parse da query string e, a partir dos parâmetros da query string, eu sei que no parâmetro "q" eu tenho o resultado, eu tenho a URL deste resultado. Agora, esse código, para mim, está suficientemente refatorado.

[03:37] Eu acho que eu já cheguei em um ponto que eu consigo bater o olho nele, ler o código sem comentários e entender o que ele faz. Então aqui nós temos algumas técnicas sendo implementadas e a principal foi a primeira, que nós fizemos, que foi a de não reatribuir um valor a um parâmetro, ou parameter reassignment, se eu não me engano é esse o nome em inglês. Só que, além disso, nós também trabalhamos com renomear variáveis.

[04:03] Então, recapitulando, a ideia é não reatribuirmos algum valor a um parâmetro, porque, de novo, isso pode confundir o código que vem depois. Nós podemos achar que estamos manipulando o parâmetro que nós recebemos, mas, na verdade, estamos manipulando uma variável que já foi atribuída, que foi substituída, e isso pode causar problemas silenciosos, que é o pior tipo de problema.

[04:25] Então, nesse cenário, nós corrigimos esse problema simplesmente introduzindo uma nova variável. Agora, ao invés de utilizarmos direto a googleUrl, estamos utilizando a resultUrl, ou seja, introduzimos uma variável para resolver aquele problema, além de fazer algumas renomeações aqui, para deixar o código mais claro.

@@03
Para saber mais: Nome do parâmetro

Quando nós renomeamos o parâmetro do método parseUrl o PHPStorm nos emitiu um alerta. Sempre que a IDE nos avisa de algo, significa que alguma melhoria pode ser feita, ou algum erro no futuro pode acontecer.
Nesse caso, nós poderíamos ter um errinho bem chato nas versões mais novas do PHP que é explicado em detalhes aqui:

Novidades do PHP 8 - Named Arguments | Dias de Dev

https://www.youtube.com/watch?v=epla4NyobjU

@@04
Método por objeto-método

[00:00] E aí, pessoal? Bem-vindos de volta. Nós começamos a mexer um pouco no algoritmo das coisas, mexendo em espaços um pouco maiores de código, não trocar uma linha por outra, um método por uma variável, mas sim mexer no geral. Por exemplo, não atribuir a variável.
[00:15] Agora voltando aqui, falando em algoritmos, quando damos uma olhada na nossa classe de crawler, nós temos a parte de acessar o Google em si, pegar os resultados e tal, só que quando temos o elemento que representa um resultado, nós temos um código relativamente grande.

[00:42] Então talvez seja interessante tirar esse algoritmo, que está maior, desta classe, porque se formos inclusive refatorar esse método, nós teremos um código bastante grande, teremos bastante trabalho para fazer. Este um algoritmo complexo dentro de uma classe que talvez ele não devesse estar.

[01:02] Aqui já começamos a analisar a necessidade de uma classe especializada para essa tarefa, para fazer o parse de um elemento do dom. Aqui vamos criar uma classe chamada dom element parser, que fará o parse de um dom element. Vamos lá, vou fazer um "DomElementParser" e eu ainda não sei como eu vou implementar ela, porque eu vou copiar o código, mas eu sei que eu terei uma função, um método chamado public function parse.

[01:31] Esse método, eu acredito que vai retornar um resultado, suponho eu que seja isso que ele retorna. Mas vamos nessa, vamos ver como isso é feito. Eu vou pegar esse código e eu já estou vendo que eu vou precisar de um outro método, o createResult. Vamos lá, deixa eu, na verdade, pegar só o corpo. Vou pegar o corpo do código.

[01:51] Recortei e vamos colar o corpo dele. O PHP Storm já me ajuda, ele vai importar isso tudo para mim. Importou.

[02:02] Aqui eu não preciso chamar de DomCrawler, eu posso chamar só de Crawler, porque eu não tenho outra classe com esse mesmo nome. Vamos nessa, eu preciso receber esse elemento, que é o Result. Vamos lá, public function parse(\DOMElement $result). E eu acho que é interessante renomearmos esse parâmetro, mas vamos com calma, chegaremos lá.

[02:20] Vamos trazer agora esse método createResult. Vou recortar ele e vou trazer para cá, porque ele também será necessário aqui. Vamos receber esse DOMElement por parâmetro aqui e show de bola.

[02:39] Teoricamente temos tudo o que precisamos, menos esse parseUrl, então vamos lá. Aqui, ele simplesmente chama o método de proxy. Deixa eu copiar isso e remover esse método. E aqui - eu nem preciso copiar, eu só terei um proxy também e vou chamar o método parseUrl.

[03:02] Então eu sei que eu vou precisar do proxy, perfeito. Se eu preciso do proxy, vamos receber ele no construtor. E aqui, se eu estivesse usando o PHP 8, eu poderia fazer isso, (private GoogleProxyInterface $proxy).

[03:16] No meu caso, aqui eu estou usando o PHP 8, então eu vou deixar essa sintaxe assim, para ser um pouco mais rápido, um pouco mais fácil. Eu já tenho aqui o nosso proxy, que tem o método parseUrl, então teoricamente esse método já está pronto, esse aqui está ok também.

[03:31] Agora vamos corrigindo os problemas que criamos aqui. Vamos ver o que temos que fazer. Teoricamente aqui eu não preciso mudar nada, pelo menos por enquanto. Beleza, vamos no crawler e usar, ao invés de chamar a função, o método parseDomElement dessa classe, eu vou criar um parse, então $domElementParser.

[03:54] $domElementParser = new DomElementParser(), aqui passando o ($this->proxy) por parâmetro, porque ele também precisa do proxy. Aqui, ao invés de chamar o parseDomElement, eu vou chamar o $domElementParser->parse.

[04:06] Ou seja, eu estou fazendo o parse desse elemento, usando essa classe específica. Então nós tiramos muito código da nossa classe, código que podia ser extraído, para uma classe específica. Nós criamos uma classe que terá um método só para isso. Então o que fizemos foi substituir um método por um objeto que tem esse método.

[04:26] Esse é o método dessa técnica, é a substituição de método por objeto método, ou método de objeto. Então pegamos o método da nossa classe, que realizava alguma função, alguma tarefa, só que essa tarefa estava complexa, podia ser difícil de refatorar dentro da própria classe, então extraímos ela para um outro local.

[04:46] Um outro momento, onde é muito comum fazer esse tipo de tarefa, é quando - imagine que temos aqui ainda aquele dom element parser e neste método, ou nessa função, temos várias variáveis locais, que utilizamos em vários pontos do algoritmo, tornando difícil a extração de métodos específicos.

[05:05] Então, nesse momento, também é muito comum extrairmos esse método para uma classe, para um objeto método, ou seja, para uma classe que terá esse método, que realiza essa ação específica. Nesse caso, acabamos tendo uma classe, um serviço bem específico, mas isso não é um problema, isso, na verdade, é uma vantagem, porque se ele é bem específico, ele tem uma única responsabilidade, ele inclusive é até um pouco mais fácil de testar.

[05:29] Então poderíamos inclusive criar agora um teste para essa classe, para garantir que o parse de um elemento do dom de resultado é feito corretamente, de forma individual. Só que já estamos testando bastante a nossa classe crawler, então não vou criar testes novos aqui. A última coisa que eu vou fazer antes de rodar os nossos testes é que eu vou alterar $result para $resultDomElement.

[05:57] Para mostrar, para deixar claro que isso é um elemento do dom que representa um resultado. Com isso tudo, eu posso rodar, no terminal, vendor/bin/phpunit e se tudo der certo e eu não fiz nenhuma besteira, os nossos códigos devem continuar passando, porque, de novo, eu não alterei nenhuma assinatura de método, eu não mudei o funcionamento de nada, eu só extraí um código.

[06:17] Então está lá, deixa eu minimizar o terminal e deixa eu voltar para o nosso crawler. Nós tínhamos mais de 160 linhas quando começamos com esse projeto, agora já estamos com algo um pouco mais aceitável, já que é um número abaixo de 100. Então isso é um código muito mais aceitável para uma classe.

[06:34] De novo, ainda dá para melhorarmos bastante esse método, que está grande, só que ele já está bem melhor do que antes. Como estamos falando dessa parte de algoritmos um pouco maior, essa parte maior de algoritmos, e não mudar uma variável, um método, vamos falar exatamente sobre isso, sobre mudar o algoritmo, a implementação de algo.

[06:56] Como isso funciona? Porque em vários momentos nós lançamos exceção. Talvez pudéssemos ter uma outra forma de controlar o fluxo sem lançar a exceção. Enfim, vamos conversar bastante, com mais detalhes, no próximo vídeo.

@@05
Desvantagem

Neste vídeo nós terminamos com uma classe a mais, bem coesa e focada em uma única tarefa, o que é um ótimo ponto. Mas praticamente todas as técnicas de refatoração possuem um ponto negativo.
Qual a desvantagem de extrair um método para um objeto-método?

A nova classe é muito específica, resolvendo só um problema.
 
Alternativa errada! Isso é um ótimo sinal.
Alternativa correta
A nova classe está muito acoplada com a pré-existente.
 
Alternativa errada! Isso é sim um problema, mas não é um problema inerente desta técnica de refatoração. Este código já estava acoplado antes, só não estava em uma classe.
Alternativa correta
Nós criamos uma nova classe, aumentando a complexidade geral do projeto.
 
Alternativa correta! Antes o que era uma única classe, agora são 2. Isso significa que precisamos escrever mais testes, há mais pontos de falha, etc. Mas sem dúvida a flexibilidade também é maior. Sempre que falamos de qualidade de código, falamos de escolhas. Precisamos pesar as vantagens e desvantagens.

@@06
Substituir algoritmo

[00:00] E aí, pessoal? Bem-vindos de volta. Como eu falei, agora estamos mexendo mais no algoritmo mesmo, em como fazemos as coisas. Uma coisa que me incomoda muito nesse código é de lançar uma exceção para simplesmente depois fazer um log dessa exceção.
[00:17] Na verdade, eu nem preciso disso, eu só quero saber se é um resultado válido e, se não for, eu posso decidir fazer um log ou posso simplesmente ignorar, que é o que eu quero. Só que, neste caso, como eu estou lançando uma exceção para ignorar isso, eu preciso ter esse bloco de catch vazio e isso é muito feio.

[00:34] Acredito que você concorda comigo. Então, o que eu quero fazer, é mudar, alterar o algoritmo de parse para que ele não lance exceções mais. Para o meu PHP Storm parar de reclamar, já que eu estou usando o PHP 8, eu vou informar para ele: eu estou usando o PHP 8, então essa sintaxe é válida.

[00:53] Mas, vamos lá, continuando, o que eu quero fazer não é mais retornar um resultado ou lançar uma exceção. O que eu quero fazer é retornar algo que talvez tenha um resultado, talvez não tenha, pode ser nada, pode ser vazio. Então quando falamos desses talvez, quando temos esse tipo, essa palavra talvez, podemos fazer de duas formas.

[01:14] Uma, a mais comum, é retornar nulo, por exemplo. Posso retornar aqui e simplesmente retornar nulo. Deixa eu inclusive selecionar todos os lugares que lançam essa exceção, que se não me engano são quatro aqui. Então eu posso vir aqui e simplesmente return null;. E aqui eu retorno o resultado válido. Dessa forma, no local onde eu recebo, eu posso verificar se não for nulo, if (!is_null(value: $parsedResult), então eu adiciono aqui.

[01:53] Então essa é uma forma de resolver o problema e aqui eu já posso me livrar deste try catch. E posso trazer isso de volta para cá.

[02:01] Então deixa eu rodar os testes e garantir que tudo continua funcionando, vendor/bin/phpunit, vamos garantir que eu não quebrei nada. Só que enquanto os testes vão rodando, eu quero chamar a sua atenção para esse detalhe aqui.

[02:14] Eu preciso me lembrar de verificar se é nulo ou não. Perfeito, está tudo passando aqui. Ok, o PHP pelo menos nos informa que esse valor pode ser nulo, dessa forma já utilizando alguma IDE, ela vai me avisar que eu preciso verificar alguma coisa. Mas, mesmo assim, essa verificação é muito fácil de esquecer. Se eu tento simplesmente fazer isso aqui, repare que eu não recebo nenhum tipo de erro, nenhum tipo de aviso.

[02:43] Isso é um grande problema. Então, o que eu quero fazer? Eu quero usar um tipo que significa exatamente isso: talvez tenha valor, talvez não tenha. Isso, no mundo da programação funcional, é conhecido como uma mônada, chamada maybe ou optional. Inclusive existem alguns conteúdos deste tema de programação funcional.

[03:04] Aqui na Alura, temos um treinamento só sobre programação funcional com PHP, vale a pena dar uma olhada. E eu vou deixar no Para Saber Mais uma explicação um pouco mais resumida dessa mônada de optional ou maybe. Mas, vamos usar um pacote para nos ajudar com isso. Esse é o primeiro que eu encontrei “yitznewton/maybe-php”.

[03:22] Não sei se é o melhor, mais performático, mas eu peguei o primeiro dessa lista e vamos instalar ele. Colei aqui no terminal e no meu caso, ele já está instalado. Instalado este pacote, o que vamos fazer? Nós não vamos mais retornar um resultado ou nulo, o que vamos fazer é retornar um Maybe, talvez tenha valor, talvez não tenha, vamos ver isso.

[03:45] Então aqui eu posso retornar, ao invés de nulo, eu vou retornar um talvez que encapsula esse nulo. Deixa eu desfazer aqui e mexer em tudo de uma vez só. Vamos lá, aqui eu vou encapsular esse nulo com um new Maybe, ou seja, talvez tenha um valor aqui dentro.

[04:02] Como eu estou passando o nulo, significa que não tem valor. Inclusive eu der uma olhada no código, repare que é isso o que ele faz, ele verifica se é alguma coisa ou não, através dessa verificação se é nulo.

[04:15] Mas, não vamos entrar em detalhes no código da biblioteca, vamos continuar aqui. Eu não posso mais retornar o resultado diretamente, eu vou retornar um Maybe(), ou seja, talvez tenha um resultado ali dentro, mas talvez não. Agora, quando eu recebo isso, eu não posso adicionar um resultado de um maybe, de algo que pode estar lá. Então o que eu vou fazer?

[04:38] Agora não é mais o resultado, é um talvez tenha um resultado, é uma mônada que pode existir um valor lá dentro ou não. Com isso, a partir dessa mônada, desse maybe, eu posso chamar um método chamado select(). Esse select será executado caso eu tenha valor.

[04:46] Então aqui, neste valor, eu sei que eu tenho o result, então $parseResult, deixa eu fechar isso, e pronto. Ou seja, esse método select, faltou um parênteses e sobrou outro, agora sim.

[05:10] Então esse select vai fazer o que? Ele executa essa função caso possua algum valor nessa mônada. Se não tiver nenhum valor lá, ele não faz nada. Essa função será executada só quando tiver algum valor lá. E ela recebe, por parâmetro, o valor. Então eu estou adicionando aqui o tipo, eu estou informando que isso é um resultado, e estou adicionando na minha lista de resultados, eu estou adicionando este resultado parseado.

[05:35] Lembrando que isso é aquela sintaxe de short closures, ou arrow functions do PHP. Tem vídeo aqui na Alura+ sobre essa sintaxe, caso você não conheça. Mas, basicamente, isso aqui é uma função anônima, como já escrevíamos antigamente. Teoricamente está tudo certo e isso aqui, para mim, está muito mais claro.

[05:53] Eu sei que mesmo que eu não conheça essa tal de mônada, eu sei que eu precisarei entender esse tipo e eu garanto que sempre, todo mundo que for utilizar esse método parse, será obrigado a verificar se o valor existe ou não. Eu não deixo a responsabilidade para o código que está usando, colocar ou não colocar o if.

[06:11] Eu assumo a responsabilidade aqui, quando eu informo: eu posso te retornar o valor ou posso não retornar nada. Então, através dessa mônada maybe eu obrigo quem estiver utilizando este retorno, a verificar se o valor existe ou não, a pegar o valor de forma indiscriminada, mas assumindo a responsabilidade, enfim.

[06:32] Teoricamente isso deve continuar funcionando, então vamos executar os testes. Enquanto os testes vão executando, o que eu fiz aqui foi basicamente substituir o algoritmo. Normalmente a substituição de algoritmo também não modifica a assinatura do método.

[06:51] Ele não muda retorno, não muda os parâmetros, ele muda somente a implementação, isso é o mais comum. Só que, nesse cenário, eu achei justo, eu achei que fazia mais sentido mudar a interface desse método, mudar o que ele retorna, porque dessa forma eu garanto que quem usa ele usa de forma mais segura.

[07:11] Então ao invés de retornar um resultado ou nulo, que o PHP me permitiria isso sem problema, eu estou utilizando um pacote a mais para garantir que quem buscar esse resultado, quem tentar parsear um resultado, sabe, com certeza, que pode não vir nada aqui. Dessa forma eu paro de usar exceções como controle de fluxo, o que é uma má prática, e eu vou deixar um Para Saber Mais aqui falando disso.

[07:33] E eu tenho um algoritmo bem mais simplificado, tanto neste ponto onde eu não lanço mais exceções, quanto no ponto onde eu uso e não preciso mais fazer um try catch e deixar o catch vazio, ou adicionar algum log. E, claro, se eu quisesse fazer um log aqui no caso de não ter nada, eu ainda posso. Eu tenho a opção de pegar um valor ou executar um callback.

[07:58] Então eu pego um valor, se esse valor existir, senão, eu faço um log aqui dentro, isso também é possível. Mas não vou fazer isso, porque exatamente essa não é a minha intenção, é não fazer o log. Mais um algoritmo simplificado, agora eu acredito que esse método está bem mais legível.

[08:12] De novo, ele cabe em uma tela só, até quando determinada folga, toda a implementação dele. Sempre dá para melhorar, então temos que cuidar para não ficar em uma refatoração infinita.

[08:25] Só que existem outros detalhes que ainda me incomodam. Por exemplo, vamos dar uma olhada no nosso proxy. Ele é responsável por buscar uma resposta HTTP e por fazer um parse de uma URL. São coisas muito diferentes na mesma interface, no mesmo tipo, com a mesma responsabilidade, então isso está me incomodando bastante, eu acho que está na hora de atacarmos esse problema, mas no próximo capítulo.

@@07
Para saber mais: Referências

Diversas referências foram citadas neste vídeo, então vou deixar alguns links aqui para você complementar seus estudos:
Nova sintaxe no construtor do PHP 8: https://www.youtube.com/watch?v=XJCSQ2nWRrQ
O que são mônadas: https://www.youtube.com/watch?v=1FdK8adSo4Y
Short closures PHP: https://cursos.alura.com.br/novidades-do-php-7-4-arrow-functions-c130
Exceções como controle de fluxo: https://wiki.c2.com/?DontUseExceptionsForFlowControl

https://www.youtube.com/watch?v=XJCSQ2nWRrQ

https://www.youtube.com/watch?v=1FdK8adSo4Y

@@08
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@09
O que aprendemos?

Nesta aula:
Vimos que não devemos sobrescrever parâmetros
Aprendemos a extrair o que é chamado de objeto-método
Conhecemos um conceito de programação funcional
Substituímos o algoritmo de um método

#### 15/02/2024

@05-Extraindo classes

@@01
Projeto da aula anterior

Caso queira, você pode baixar aqui o projeto do curso no ponto em que paramos na aula anterior.

https://github.com/alura-cursos/google-crawler/archive/refs/tags/aula-4.zip

@@02
Extrair classes

[00:00] E aí, pessoal? Bem-vindos de volta a mais um capítulo deste treinamento, onde estamos fazendo algumas refatorações bem interessantes em códigos reais. Vamos lá, eu quero agora modificar essa GoogleProxyInterface. Ela não está legal porque ela é uma interface que faz muita coisa, é uma interface que nos obriga a saber realizar uma requisição HTTP e devolver a resposta.
[00:22] E é uma interface que nos obriga a saber parsear uma URL. Então eu quero separar esses dois. Deixa eu abrir o menu lateral, vamos ver onde eu vou organizar isso. Aqui em "Proxy", eu tenho - opa, eu cliquei em alguma coisa que eu não deveria ter clicado, mas tudo bem. Eu terei "HttpClient", um cliente HTTP que vai fazer o que tem que fazer, seja lá como ele vai realizar essas chamadas.

[00:48] E o URL parse. Então novo diretório, "UrlParse". Então cada proxy terá uma implementação de um cliente HTTP e de um parseador de URL. Eu terei uma interface no "HttpClient", vamos lá, será "GoogleHttpClient", ou alguma coisa do tipo, você pode pensar em um nome melhor. O que essa interface terá é esse método aqui.

[01:14] Vamos lá, tem esse método aqui. Então ela espera uma string, que é uma URL, e devolva uma resposta da PSR. Caso você não saiba o que é uma PSR, no treinamento de MVC nós falamos bastante sobre esse assunto. Mas vamos lá, continuando, eu terei aqui uma "GoogleUrlParser", que também é uma interface. Nessa interface eu terei esse método aqui.

[01:46] Para quem conhece os princípios solid, o que eu estou fazendo é basicamente aplicar aquele inner face aggregation principle, o princípio I. Se você não conhece solid tudo bem, mas fica a dica para você estudar, que é um assunto muito interessante. Perfeito, deixa eu remover isso, onde está o delete? Aqui.

[02:06] Ou poderia copiar aquela documentação, que eu acho que é interessante, mas vou deixar para lá. Então, agora eu quero implementar pelo menos esse de "NoProxy". Vamos lá, "NoProxyGoogleHttpClient". Eu vou pegar a implementação do "NoProxy", que vai pegar a resposta HTTP. Vamos lá, vou copiar isso. Vou colar na nova classe, ok. Aqui eu implemento um http client, um GoogleHttpClient. Perfeito.

[02:44] De novo, existe uma forma melhor de fazer isso, eu não deveria criar esse cliente aqui diretamente, mas sim recebendo ele por parâmetro aqui, eu deveria receber uma ClientInterface aqui, esse é o ideal.

[02:57] Mas, eu não vou fazer isso para não alterar coisa demais. Vou deixar inclusive isso de desafio para você, para que você implemente desta forma, com injeção de dependência. “Vinicius, não sei o que é injeção de dependência”. Também tem vídeo aqui na Alura+ falando sobre isso, dê uma olhada lá, a Alura+ está cheia de conteúdo legal.

[03:13] Mas, continuando, vamos criar agora o nosso "NoProxyGoogleUrlParser". Perfeito, ele implementa, obviamente, o class NoProxyGoogleUrlParser implements GoogleUrlParser- deixa eu tirar isso daqui, tirar isso daqui e vou remover daqui essa implementação.

[03:36] Eu posso, inclusive, apagar esse "NoProxy", apagar essa classe - cadê o "Delete"? Pronto, classe jogada fora. Vamos nessa e adicionar a implementação. Vou importar o que tem que importar, no caso é uma exceção, perfeito. Vamos ver do que ele está reclamando, deixa eu ver aqui, o "GoogleUrlParser", eu copiei a coisa errada.

[04:01] Agora me complicou um pouco. Vamos lá, eu sei que aqui é essa interface, eu copiei aqui para o "UrlParser" a implementação errada, a interface errada. Eu vou apagar isso, sem problema, não precisamos nos preocupar.

[04:17] Então perfeito, tenho agora as coisas separadas e eu preciso que o meu crawler receba, ao invés de um proxy, que não faz mais sentido receber isso, ele vai receber um parser, então Google, primeiro o client - pode ser o GoogleUrlParser, e o GoogleHttpClient. Então eu quero receber esses dois. Vamos lá, vou inicializar isso aqui direto, não vou mais ter esse padrão.

[04:50] Dessa forma, eu preciso modificar todos os lugares que fazem requisição, que fazem esse tipo de coisa, para utilizar o nosso httpClient, nesse cenário, aqui ele vai passar direto o urlParser, se eu não me engano, o nosso parser. E aqui ele precisa receber, obviamente, um public function __construct(private GoogleUrlParser $proxy). Deixa eu ver se nada quebrou.

[05:12] Perfeito, teoricamente tudo funcionando. Só que agora nossos testes não passam esses dois objetos novos, eles não passam esses dois, por parâmetro, então nossos testes vão quebrar. Só que o que eu fiz? Vamos recapitular, vamos focar aqui no ponto da refatoração.

[05:32] Eu peguei um código que estava em um lugar só, naquele nosso "NoProxy" e extrai esse código que não fazia sentido estar lá, por causa de muitas responsabilidades, eu extrai para uma classe específica. Eu realizei esse processo duas vezes, então do "NoProxy" eu extrai uma classe "NoProxyGoogleHttpClient" e extrai uma outra classe, "NoProxyGoogleUrlParser".

[05:55] Essa extração de classes é uma refatoração muito comum. Quando temos um pedaço de código que parece não pertencer àquele local, nós extraímos para uma classe diferente. Muito semelhante ao que fizemos em "DomElementParser".

[06:09] Nesse caso, nós extraímos algo específico, que é uma chamada de método, e nós extraímos, então essa é o nome, no catálogo de refatorações, essa ideia é um pouco diferente, mas extração de classes um pouco mais genéricas também é uma refatoração muito comum. Só que, de novo, voltando ao nosso código, eu não quero receber esses dois parâmetros.

[06:30] Eu quero simplificar um pouco a criação do meu construtor, a criação desse meu objeto no construtor. No próximo vídeo nós vamos implementar um padrão de projeto que é bastante mal compreendido, inclusive tem vídeos, tem cursos de padrão de projetos aqui, na Alura, nós aprendemos, estudamos sobre ele, e agora vamos ver isso, na prática.

@@03
Por que 2?

Neste vídeo nós separamos as interfaces de Cliente HTTP e Parser de URL em 2 interfaces específicas.
Por que separar as responsabilidades em interfaces diferentes?

Porque assim podemos ter classes específicas para realizar cada tarefa
 
Alternativa correta! Com esta refatoração nós passamos a aderir os princípios S e I de SOLID. Nós separamos as interfaces (I) e fizemos com que cada classe tivesse uma responsabilidade mais bem definida (S).
Alternativa correta
Porque com mais interfaces, nossas classes podem performar melhor
 
Alternativa correta
Porque quanto mais interfaces no projeto, melhor

@@04
Abstract factory

[00:00] E aí, pessoal? Bem-vindos de volta. Eu queria bater um papo bem rápido, super resumido, sobre o que fizemos no último vídeo, porque extrair uma classe é uma tarefa bastante comum, mas pode ser feita de várias formas. Como nós vimos naquele cenário, nós pegamos uma classe e transformamos em duas.
[00:15] Mas, uma outra forma muito comum de aplicar essa extrair classe, essa refatoração de extrair classe, é nesse cenário exemplificado aqui. Imagine que eu tenho uma classe Pessoa, ela tem um nome, tem um código de área do escritório, tem um número do escritório, e tem como buscar o número do telefone.

[00:34] Então tem algumas coisas aqui, nessa pessoa, e vemos que algumas coisas não encaixam, não parecem fazer parte da pessoa em si. Então extraímos uma nova classe, por exemplo, número de telefone e então temos o código de área e o número, e faz muito mais sentido nesse cenário.

[00:49] então extrair classe, nós podemos usar em alguns contextos. Quando algo parece que não faz sentido estar naquela classe, nós extraímos para uma classe diferente e podemos usar essa implementação de formas diferentes também. Aqui, neste cenário, quando eu extrai uma classe, Pessoa passa a ter um número de telefone, então temos uma associação. Já no cenário que nós aplicamos, a classe original deixou de existir para dar lugar a essas duas novas.

[01:16] Mas, vamos lá, o que vamos implementar agora é o que é conhecido como abstract factory, ou seja, uma fábrica abstrata. O que vamos fazer é criar uma interface que permite a criação de parser e de cliente HTTP, ambos para o Google, óbvio. E teremos uma implementação de no proxy factory, teremos uma k proxy factory, common proxy factory e etc.

[01:41] Então vamos lá, eu posso criar um novo namespace para fábricas, mas eu vou deixar na raiz mesmo. "ProxyFactory", ou melhor ainda, "ProxyAbstractFactory". Nós podemos pensar em um nome melhor, para não usar o nome do padrão de projeto na classe, mas tudo bem, isso não vai atrapalhar em nada aqui.

[02:01] Como eu comentei, nós estudamos esse padrão de projeto no treinamento de padrões de projetos, mas se você não fez esse treinamento, não se preocupe, que você vai entender o que eu vou fazer. Eu vou criar aqui a minha interface, que diz que eu consigo criar public function createGoogleHttpClient():.

[02:23] Isso nos retorna um : GoogleHttpClient. Eu também posso criar, então public function createGoogleUrlParser():. Isso vai retornar, obviamente, um : GoogleUrlParser. Simples assim. Aqui eu vou renomear para GoogleProxyAbstractFactory, acho que fica um pouco mais interessante

[02:45] Agora vamos para as implementações. Eu terei aqui o nosso "NoProxyAbstractFactory", porque essa fábrica, ela sabe criar algumas coisas. Vamos lá, implement GoogleProxyAbstractFactory. E o que temos que implementar? Esses dois métodos.

[03:09] Então um será simplesmente retornar o nosso return new NoProxyGoogleHttpClient(). De novo, se estivéssemos recebendo o cliente HTTP, aquele do goose ou aqui, por parâmetro, eu simplesmente passaria o cliente aqui. Mas, continuando, vamos ver qual é o problema aqui. Não é problema nenhum. Eu posso retornar return new NoProxyGoogleUrlParser. Eu criei aqui a minha fábrica abstrata de sem proxy nenhum.

[03:43] Agora, o que eu posso receber no nosso crawler, ao invés de receber esses dois, eu vou receber, na verdade, uma GoogleProxyAbstractFactory. Inclusive eu posso voltar com aquela nossa regra de permitir que isso seja nulo. Então vamos criar aqui um private googleHttpClient $httpClient.

[04:10] E teremos um private GoogleUrlParser $urlParser;. Aqui vamos criar se if ($factory === null) eu vou criar de uma forma, senão, eu vou simplesmente usar a factory.

[04:32] Então vamos lá, o nosso $this->httpClient = $factory->createGoogleHttpClient(), e o nosso $this->urlParser será o da factory também, = $factory->createUrlParser(). Caso contrário, eu posso eu mesmo criar. Ou melhor ainda, olha só, eu posso dizer que $factory = new NoProxyAbstractFactory() e eu não preciso adicionar mais nada, eu posso utilizar direto aqui.

[04:55] Nós já temos os nossos objetos inicializados. Teoricamente precisamos alterar menos testes agora. Vamos ver, sim, aqui eu utilizei errado, http client e aqui é url parser.

[05:13] Teoricamente o nosso código não tem mais problemas, vamos ver. Posso remover essas dependências antigas aqui, que eu não estou mais usando. Vamos lá. Aqui deixa eu quebrar aqui, tirar aqui, quebrar essa linha para ficar um pouco mais claro, e para ficar um pouco mais claro.

[05:31] Na verdade, para melhorar mais ainda, todos esses namespaces agrupados, eu prefiro separar eles, eu acho que vai ficar um pouco melhor, um pouco mais claro, no máximo eu poderia agrupar tudo o que está aqui em proxy, mas eu acho que não vale a pena.

[05:45] Continuando, só falta atualizarmos os nossos testes. Mas vamos recapitular o que eu fiz: eu tinha uma classe que fazia requisições HTTP e realizava o parse de uma URL. Ela tinha muita responsabilidade e eu extraí para duas classes diferentes, a que faz requisições HTTP e a que faz o parse de URL.

[06:05] Para que eu não precise ficar recebendo vários parâmetros por argumento, sendo que esses parâmetros estão diretamente relacionados, não faz sentido eu realizar uma requisição HTTP com o cliente do common proxy e tentar fazer o parse da URL com o no proxy. Isso não funcionaria. Então, eu criei uma abstract factory para criar esses objetos de forma agrupada, a partir de um local único.

[06:26] Agora, o que vamos fazer é alterar nossos testes. De novos, vamos deixar aqueles testes que usam proxies de lado, mas eu deixo de novo mais um desafio para você: também criar uma fábrica desse common proxy, para conseguirmos continuar com aqueles testes. Mas eu não vou fazer aqui, eu vou utilizar somente - vamos no nosso default crawler.

[06:49] Esse commonProxy eu vou marcar como pulado também, eu vou pular esse teste aqui, 'Implementation outdated', e vou atualizar esse de cima. O nosso $crawler = ner Crawler();, como ele continua com o parâmetro podemos ser nulo, eu vou deixar esse parâmetro nulo e estamos felizes.

[07:04] Aqui, se eu não me engano, não tenho mais nenhum teste que seja realmente executado. Vamos dar uma olhada no próximo.

[07:11] Esse eu não vou passar mais o NoProxy, eu vou passar o NoProxyAbstractFactory(), e isso, como nós já vimos, não é aqui que usamos, não sei nem o porquê eu deixei de atualizar isso, não sei o que aconteceu nesse cenário. Acho que nem estávamos utilizando esses testes. Mas vamos lá, eu vou receber aqui, no local correto. Agora sim. Vou quebrar a linha, para ficar um pouco melhor, para ficar um pouco melhor e para ficar um pouco melhor. Agora sim.

[07:41] Aqui embaixo a mesma coisa, vamos lá, aqui não recebe mais um proxy, isso recebe um NoProxyAbstractFactory.

[07:50] Teoricamente tudo certo, deixa eu garantir que eu não preciso remover nenhuma importação aqui, essa. Teoricamente já atualizamos esse teste. Vamos ver se temos algum teste de unidade? Temos esses aqui, podemos simplesmente marcar esses markTestSkipped('Implementation outdated'). Então a implementação está desatualizada, não vamos rodar esses testes mais.

[08:16] Vamos deixar eles para depois. Eu deixo vocês corrigirem eles, se for o caso, ou caso vocês queiram. Vamos continuar. Teoricamente, acredito que temos tudo certo. Aqui um detalhe importante, isso é o principal, os testes de unidade.

[08:34] NoProxyAbstractFactory, vamos lá, em todo lugar que estiver criando isso eu vou passar o NoProxyAbstractFactory. Vamos ver se aqui precisamos criar alguma coisa, aqui um proxy. Aqui nós temos um mock.

[08:52] Nós temos um dublê de teste. Se você não entender o que eu vou fazer aqui, não tem problema, nós temos um treinamento de testes, mas eu vou atualizar esse teste para garantir que ele vai continuar passando. Vamos lá, eu vou criar aqui um GoogleHttpClient e é isso que ele vai retornar.

[09:10] Teoricamente é isso, vamos alterar para $googleHttpClient. Eu vou criar também um proxy. Deixa ver, se eu preciso daquele parse URL, acho que não. A princípio é isso.

[09:28] Posso então criar aqui uma $factoryMock = $this->createStub() de abstract factory e eu vou dizer que o método method()- qual é o nome daquele método? O método que temos na interface, que retorna, que cria, na verdade, o nosso cliente HTTP, é esse aqui. Então esse método vai retornar, return, aquele nosso --willReturn($googleHttpClient).

[10:05] O que eu estou fazendo aqui, eu estou criando essa fábrica de mentiras para passar aqui no nosso teste, então $factoryMock, para que nesse teste aconteça o que tem que acontecer. No caso, lançar uma exceção de resultado inválido, esse HTML está inválido.

[10:19] Vamos ver onde isso é lançado. Isso é lançado no cenário quando tentamos criar o nosso resultado e não tem nenhum resultado para ser encontrado, então entendemos que nenhum elemento foi parseado. Teoricamente os nossos testes estão atualizados. Se eu não dei nenhuma bobeira, tudo deve funcionar.

[10:41] Vamos lá, mais testes pulados, e temos alguns erros aqui. Eu vou finalizar esse vídeo aqui, que já está longo, corrigir esses erros e voltar com a solução para você, bem explicada, para você não ficar me assistindo buscar esses erros.

@@05
Padrão de projeto

Neste vídeo nós implementamos o padrão de projeto Abstract Factory.
Qual o propósito de uma fábrica abstrata, segundo este padrão de projeto?

Criar objetos vindos de classes abstratas
 
Alternativa correta
Criar objetos diretamente relacionados
 
Alternativa correta! Uma fábrica abstrata no nosso caso sabe criar um cliente HTTP que acessa o google e uma classe que faz o parse de URLs do Google.
Alternativa correta
O mesmo que um factory method: criar objetos

@@06
Extra: Outras técnias

[00:00] E aí, pessoal? Bem-vindos de volta. Muito provavelmente você percebeu antes de mim alguns detalhes. O primeiro é que aqui, a nossa interface de Google Http client, ela não tinha o método, eu não coloquei, eu fiz uma confusão e não adicionei esse método. Aqui estava o problema, por isso eu estava recebendo um aviso da IDE, que eu estava achando confuso, mas resolvi ignorar.
[00:20] Está aqui o problema. O outro detalhe é que eu estava colocando isso tudo em uma linha só. Com o PHP Storm, como já vimos, tanto no treinamento de testes quanto no vídeo aqui na Alura+ sobre dublês de teste, sobre como criar dublês sem expectativas, tem um vídeo na Alura+, nós precisamos primeiro criar o dublê e com o dublê criado, nós podemos configurar ele.

[00:43] Se eu tento configurar tudo na mesma linha, o que terá na variável é o retorno desse método, que não é o dublê. Foi uma confusão que eu fiz, mas aqui, vou até rodar de novo para você ver comigo, todos os testes agora passam.

[00:56] Então o que fizemos neste último vídeo, para recapitular, foi pegar uma classe - nesse último capítulo, na verdade - pegar uma classe de proxy e extrair uma outra classe, que faz requisições HTTP, e separar a responsabilidade. Depois pegamos o método que faz o parse de URL e extraímos para um outro local também. Então podemos até excluir a classe original.

[01:16] Mas, o foco da refatoração foi aquilo que eu te mostrei naquela imagem, de refatorar para: eu tenho uma classe, tenho alguns métodos que parecem fora de lugar, que podem ser extraídos, então eu extraio para uma classe separada. Inclusive, nos nossos treinamentos básicos de orientação a objetos, nós fizemos isso, extraindo o CPF, por exemplo, extraindo o endereço do nosso titular.

[01:37] Nós fizemos esses trabalhos, onde tínhamos determinadas informações, que talvez não devessem estar lá, e extraímos para classes específicas. Com isso, nós já modificamos bastante - aqui, só para você ver, o teste que eu tive que corrigir foi esse aqui, onde tentávamos lançar uma exceção.

[01:55] Mas ok. E aqui, o que temos de resultado final é um desafio para que você faça essa implementação aqui igual fizemos do “NoProxy” faça com “CommonProxy”também e o que eu acredito que ele, igual fizemos a do "NoProxy", que você faça com o "CommonProxy" também. E o "KProxy", eu acredito que ele já nem funcione mais, mas, caso você queira praticar, também é uma boa de implementar.

[02:13] Com isso, descomente aqueles testes, tire aquele método que marca eles como incompletos, deixa eu mostrar para vocês os testes que eu marquei como incompletos. Aqui, nesses personalizados, eu marquei o teste como pulado, na verdade, para pular esse teste, porque esse teste está desatualizado, então o código não está correto. Quando chamamos esse método do PHPUnit, ele nem executa esse método, ele vê essa linha e fala: vou sair daqui, então, nem executo. Então fica o desafio para você fazer essas implementações.

[02:50] E eu quero deixar uma referência bastante interessante para você. Nesse site, o "refactoring.guru", esse site, ele tem muita coisa interessante, nos treinamentos de padrões de projeto nós andamos bastante por ele. Ele também tem uma seção de refatoração.

[03:07] Ele tem aqui bastante coisa do que já falamos, sobre o que é refatoração, vale a pena a leitura, o que é dívida técnica, quando e como refatorar. Só que em alguns momentos eu falei de um catálogo de refatoração. Esse catálogo de refatoração vem de um livro muito famoso, “Refactoring”, e esse livro, ele provê para nós exatamente isso, um catálogo, onde nós temos um problema e esse problema é visto como code smell.

[03:31] Nós temos uma forma de refatorar, que é através das refactoring techniques ou técnicas de refatoração. Com isso, esse site montou esse catálogo para nós.

[03:41] Ele explica o que é um code smell e ele mostra vários desses code smells, o que é uma técnica e ele mostra várias dessas técnicas, separadas por categoria. E, neste treinamento, nós vimos algumas dessas. Só que repare que são muitas, e se eu fosse passar técnica por técnica, te explicando todas, eu precisaria criar mais problemas do que um projeto real tem.

[04:04] Eu precisaria ou trazer um projeto muito grande, então iríamos encontrar isso tudo, ou ficar criando problemas e acho que não ia ficar tão legal. Então a ideia por trás das refatorações foi isso que vimos neste treinamento. Existem muitas técnicas que valem a pena conferir. Esse site é bastante didático, então vou deixar no Para Saber Mais. Como já utilizamos ele em outros treinamentos, eu acho que fica legal dar uma olhada nele.

[04:29] Só para você entender, de novo, aqui nós temos os codes smells, que eles também estão separados por categoria, então códigos que deixam o nosso código muito inchado, abusadores da orientação a objetos, algumas coisas que acabam nos impedindo de fazer mudanças, que dificultam mudanças, coisas que são dispensáveis, enfim, acopladores, tem categorias.

[04:57] Dentro de cada uma dessas categorias nós temos vários code smells, vários problemas. Cada um desses problemas tem algumas soluções, que são essas técnicas de refatoração. Nós vimos alguns desses, de extrair método, fazer o inline de método, extrair variável, adicionar uma variável, fazer o inline de uma variável temporária.

[05:17] Tem bastante coisa interessante, são coisas bem pequenas, deixa eu clicar em qualquer um aqui. Esse nós não vimos, “Feature Envy”.

[05:24] Tem os sinais e sintomas, e ele mostra uma imagem ilustrativa, tem a descrição bem rápida e tem o tratamento, ou seja, quais refatorações podemos utilizar. A partir disso ele mostra vantagem e desvantagem e, inclusive, algumas dessas techniques, alguma dessas técnicas, por exemplo, mover método, elas mostram aqui em imagens a forma de implementação.

[05:47] E algumas delas tem até um código de exemplo, você consegue ver em diversas linguagens, então vale a pena conferir. Para quem prefere leitura mais formal, eu vou deixar no Para Saber Mais o link do livro, porque o livro que originou esse catálogo, e é bastante extenso.

[06:03] Eu confesso que eu achei ele um pouco maçante na época que eu li, eu li já tem alguns anos, então eu achei ele um pouco cansativo de ler, por isso eu estou apresentando como a primeira referência esse site, que é bem mais dinâmico, ele é bem mais didático, mais interativo. Mas o livro ainda é a referência oficial,

[06:22] Então eu vou deixar o link no Para Saber Mais, caso você se interesse, recomendo, que é uma leitura válida, embora seja um pouco maçante, pelo menos na minha opinião. Enfim, já estou me alongando demais, essa foi toda a ideia por trás da refatoração, e no próximo vídeo eu venho para fazer aquela conclusão e dar aquele abraço.

@@07
Para saber mais: Referências

Nós vimos durante o treinamento diversas técnicas de refatoração, mas isso é só o começo. Existem inúmeras outras técnicas para tornarmos nosso código melhor.
Se você quiser se aprofundar nos conceitos, pode ler o livro considerado oficial que apresenta esse catálogo de refatorações: https://amzn.to/3m32rBC

Caso prefira um conteúdo um pouco mais interativo, este site é muito interessante: https://refactoring.guru/refactoring/

@@08
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com os próximos cursos que tenham este como pré-requisito.

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@09
Projeto final do curso

Caso queira, você pode baixar aqui o projeto completo implementado neste curso.

https://github.com/alura-cursos/google-crawler/archive/refs/tags/aula-5.zip

@@10
O que aprendemos?

Nesta aula:
Aplicamos o princípio I de SOLID
Extraímos classes mais coesas
Aplicamos o padrão Abstract Factory
Conhecemos as referências sobre refatoração

@@11
Conclusão

[00:00] E aí, pessoal? Parabéns por chegarem ao final deste treinamento, onde vimos algumas técnicas de refatoração com PHP. Refatoração é um assunto muito extenso, nós só molhamos o nosso pé nesse oceano. Para trabalharmos um pouco com refatoração, nós usamos um projeto real, que inclusive está até descontinuado, ou seja, é um projeto relativamente antigo, e por isso tínhamos bastante coisa que podia melhorar.
[00:26] A partir desse projeto nós vimos algumas técnicas muito interessante, como extrair ou fazer inline de variáveis, extrair ou fazer inline de métodos, quando utilizar variáveis temporárias ou não, quando utilizar métodos e quando utilizar variáveis, extrair classes, substituir algoritmos. Inclusive aplicamos padrões de projeto, falamos sobre solid.

[00:47] Quando estudamos esse tipo de coisa, nós vemos a real necessidade de conhecer alguns padrões de projeto, mesmo que você não conheça todos, que você já tenha ouvido falar, saiba mais ou menos para que serve, a real importância de conhecer solid, porque mesmo em um projeto que não esteja com o melhor código do mundo, nós conseguimos aplicar as técnicas e melhorar, nem que seja um pouco a legibilidade, a facilidade de testar.

[01:11] Inclusive vimos a importância de testes na hora de realizar refatorações, principalmente no início, quando começamos a refatorar, alguns testes começaram a quebrar, nós íamos, arrumávamos o código. E vimos que se mudamos muito o nosso código, nós temos um trabalho extra de mudar os nossos testes também, mudar, obviamente, o código de produção que usa esse nosso código.

[01:32] Então precisamos sempre botar na balança quando vale a pena e quanto vale a pena refatorar. Mas, enfim, se em alguma parte desse processo, que eu sei que foi um curso não tão fácil assim, se em alguma parte dele ficou alguma dúvida, algo não ficou claro, não hesite, abra uma dúvida no fórum, que eu vou ficar mais do que feliz em te ajudar.

[01:52] Eu tento responder pessoalmente, sempre que possível, mas às vezes, quando eu não consigo responder a tempo, temos uma comunidade incrível de alunos, moderadores e instrutores, alguém com certeza vai conseguir te ajudar. Enfim, mais uma vez parabéns por ter chegado até o final, espero que você tenha aproveitado bastante e espero te ver em outros conteúdos aqui na Alura. Um forte abraço, tchau.