# O primeiro snippet
A única forma de aprender uma nova linguagem de programação é escrevendo programas com ela. O primeiro programa a ser escrito é o mesmo para todas as linguagens:
*hello, world*

Como nem tudo é tão simples, pra você poder criá-lo e executá-lo, você deve:
- Criar o arquivo com o código da linguagem
- Compilar o arquivo criado
- Carregar o arquivo compilado
- Executar o arquivo carregado
- Obter o output (saída) do arquivo executado

Em C, o programinha para printar (imprimir uma saída de texto no terminal) é:
```c
#include <stdio.h>

main()
{
	printf("hewwo, world!\n");
}
```

Sobre a forma de executar esse código depende do sistema operacional que você esteja utilizando.

Um programa em C, independentemente de seu tamanho, consiste em funções e variáveis. Uma função contém *statements* ("afirmações") que especificam operações computacionais para serem realizadas, e variáveis armazenam valores utilizados durante a computação. Funções em C são como subrotinas e funções em Fortran ou procedimentos e funções do Pascal. O exemplo acima é de uma função chamada "main". Geralmente você tem liberdade de dar o nome que quiser a funções, mas o nome "main" é especial - seu programa começa a execução do início pela "main" (como uma espécie de ponto de entrada/entrypoint). Isso significa que todo programa deve ter uma função "main" em algum lugar.

A "main" geralmente irá chamar outras funções para ajudar a performar seu trabalho, sendo aquelas que você criou e outras de bibliotecas que foram fornecidas pra você (através de importações, por exemplo :3)

A primeira linha do programa `#include <stdio.h>`, diz ao compilador para incluir informações da biblioteca *stdio.h* (library padrão para entrada/saída); essa linha, por exemplo, geralmente é vista na maioria dos arquivos em C.

Um método de passar dados entre funções é fornecer uma lista de valores durante sua chamada, conhecidos como *argumentos*. Os parênteses depois de uma função, como acontece em `main()`, indica que ele pode receber uma lista de argumentos (podendo estar vazia ou não).
**continuar da página 10**