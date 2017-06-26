# Guia de Estilo Git

## Ramifica��o (Branching)

- Escolha nomes curtos e descritivos:
	#bom# $ git checkout -b oauth-migtration
	#ruim# $git checkout -b login_fix
- Identificadores de tickets correspondentes num servi�o externo (p.ex. um issue do GitHub) tamb�m s�o bons candidatos para uso em nomes de ramos. Por exemplo:
	# issue do GitHub #23
	$ git checkout -b issue-15
�	Use barras para separar palavras
�	Quando v�rias pessoas estiverem trabalhando numa mesma funcionalidade, pode ser conveniente ramos de funcionalidade pessoais e um ramo de funcionalidade do time. Use a seguinte conven��o para nomes:
	$ git checkout -b feature-a/master # ramo do time
	$ git checkout -b feature-a/joao # ramo do Jo�o
	$ git checkout -b feature-a/maria # ramo da Maria
	"Merge" livremente os ramos pessoais ao ramo do time. Eventualmente, o ramo do time ser� "mergido" ao master.
�	Apague o seu ramo do reposit�rio superior depois que ele for "mergido", a n�o ser que haja uma raz�o espec�fica para n�o fazer isso.
	Dica: Use o seguinte comando quando estiver no �master�, para listar ramos "mergidos": 
	$ git branch --merged | grep -v �\*�

Commits

Cada commit deve ser uma �nica mudan�a l�gica. N�o coloque v�rias mudan�as em um �nico commit. Por exemplo, se um patch conserta um bug e otimiza a performance de uma funcionalidade, divida-o em dois commits separados.
Dica: Use git add -p para organizar interativamente por��es espec�ficas dos arquivos modificados.
N�o separe uma mudan�a l�gica �nica em diversos commits. Por exemplo, a implementa��o de uma funcionalidade e os testes correspondentes devem estar no mesmo commit.
Fa�a commits cedo e frequentemente. Commits pequenos e contidos s�o mais f�ceis de entender e reverter se algo sair errado.
Commits devem ser ordenados logicamente. Por exemplo, se o commit X depende de mudan�as feitas no commit Y, ent�o o commit Y deve vir antes do commit X.

Mensagens de Commit

Uma mensagem de commit consiste de tr�s partes distintas separadas por uma linha em branco: o t�tulo, o corpo (opcional) e o rodap� (opcional). O layout fica assim:

Feat: Ensina como escrever mensagem de commit

�s vezes, uma maior explica��o pode ser �til.

Resolves: #1234

O t�tulo se divide em 2 partes: o tipo e o assunto.

Tipo
O tipo fica contido no t�tulo e pode ser um dos seguintes:
feat: uma nova funcionalidade
fix: um conserto de bug
docs: mudan�as de documenta��o
style: formata��o, ponto-e-v�rgula faltando, etc.; sem mudan�a de c�digo
refactor: refatora��o c�digo de produ��o
test: adicionar testes, refatorar testes; sem mudan�a de c�digo de produ��o
chore: atualiza��o de build tasks, configura��o de gerente de pacotes, etc.; sem mudan�a de c�digo de produ��o

Assunto
Assuntos n�o devem ter mais de 50 caracteres, devem come�ar com uma letra mai�scula e n�o terminar com ponto. Use o modo imperativo na linha de assunto. Separe o assunto do corpo (quando houver um) com uma linha em branco.
Exemplo: #bom# Refactor subsystem X for readability #bom# Remove deprecated methods #ruim# Fixed bug with Y #ruim# More fixes from broken stuff

Corpo
Como nem todos os commits s�o complexos o suficiente para requerer um corpo, ele s� deve estar presente na mensagem quando deixar um contexto ali e agora poupar o tempo de colegas e futuros contributors. 
Use o corpo para explicar o "o qu�?" e o "por qu�?" de um commit, n�o o "como?" � o c�digo � que deve fazer isso. 
Quando escrever uma mensagem de commit, pense no que voc� mesmo precisaria saber se voc� desse de cara com o seu commit daqui a um ano.
Limite o corpo a 72 caracteres por linha.

Rodap�
O rodap� tamb�m � opcional e � usado para monitoramento/refer�ncia de issues:
Exemplo: 
Resolves: #123
Veja tamb�m: #456, #789

Merging

�	N�o reescreva hist�rico publicado. O hist�rico do reposit�rio � valioso por si pr�prio e � muito importante para que se possa dizer o que realmente aconteceu. Alterar hist�rico publicado � fonte comum de problemas para qualquer um trabalhando no projeto.
�	No entanto, existem casos em que reescrever o hist�rico � leg�timo. Esses casos s�o:
o	Voc� � o �nico trabalhando naquele ramo e ele n�o est� sendo revisado.
o	Voc� quer arrumar o seu ramo (p.ex. dar squash em commits) e/ou dar rebase dele no �master� para dar merge mais tarde.
Dito isso, nunca reescreva o hist�rico do ramo "master" ou qualquer outro ramo especial (isto �, utilizados por servidores de produ��o ou CI)
�	Mantenha o hist�rico limpo e simples. Imediatamente antes de dar merge no seu ramo:
o	Certifique-se de que ele est� conforme o guia de estilo e realize qualquer a��o necess�ria se ele n�o estiver de acordo (dar squash/reordenar commits, reescrever mensagens, etc.)
o	Dar rebase do seu ramo no ramo em que ele ser� "mergido"
Isso resulta num ramo que pode ser aplicado diretamente ao fim do ramo "master" e resulta num hist�rico muito simples.
Essa estrat�gia � mais adequada para projetos com ramos "de tiro curto". Em outros casos pode ser melhor ocasionalmente dar merge no ramo "master" ao inc�s de dar rebase nele.
�	Se o seu ramo inclui mais de um commit, n�o d� merge com fast-forward:
# bom - assegura que um commit de merge � criado
$ git merge --no-ff my-branch
# ruim
$ git merge my-branch
NUNCA d� commit em algo como �Fix linter� ou �Fix tests� (consertos de bugs/tests). Esses �fixes� devem tomar squash para os commits que os originaram.
NUNCA d� squash totalmente num ramo antes de dar merge nele, a n�o ser que o ramo inteiro (todos os commits) estejam relacionados a uma �nica mudan�a l�gica.
NUNCA use git merge master num ramo. SEMPRE use git rebase master, depois force-push, espere que o CI (sistema de Integra��o Cont�nua) liberar e s� ent�o merge ele no "master".
A CONVEN��O � sempre usar a interface web do Github para dar merge no "master" e NUNCA usar Git na linha de comando (isto �, git checkout master; git merge --no-ff branch; git push). Isso evita confus�o e esquecer de coisas muito importantes como merge sem fast-forward.

Diversos

�	Existem variados workflows e cada um tem suas for�as e fraquezas. Se um deles se ad�qua ao seu caso, depende do seu time, do projeto e do seu procedimento de desenvolvimento.
Com isso em mente, o mais importante � de fato escolher um workflow e seguir firme com ele.
�	Seja consistente. Isso � relacionado ao workflow mas tamb�m se expande para coisas como mensagens de commit, nomes de ramos e tags. Ter um estilo constistente atrav�s do seu reposit�rio faz dele mais f�cil que todos os contributors possam compreender o que est� acontecendo ao olhar o log, uma mensagem de commit, etc.
�	Teste antes de dar push. N�o d� push em trabalho feito pela metade.

Use o Senso Comum, Luke

Use o Senso Comum, Luke. SIGA ESSE GUIA! Isso � muito importante, caso contr�rio n�s n�o far�amos voc� l�-lo antes de contribuir com nossos reposit�rios.

Agradecimentos

This guide was inspired on other guides found in the community, some of which we�d like to give a special thanks:
https://github.com/agis/git-style-guide#merging
https://chris.beams.io/posts/git-commit/
https://github.com/chrisjlee/git-style-guide
