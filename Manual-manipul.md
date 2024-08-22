Esse arquivo tem o objetivo de mostrar e explicar as principais funções para manipulação de arquivos binários e os tipos de organização de arquivos binários, mostrando exemplos, seções importantes do código, vantagens e desvantagens. Todo conteúdo se sustenta na matéria de "Estrutura de Dados II" ministrada pela docente Verônica Oliveira de Carvalho da Unesp de Rio Claro.

- [Conceitos iniciais](#conceitos-iniciais)
  - [O que é um buffer](#o-que-é-um-buffer)
    - [Funcionamento de um buffer](#funcionamento-de-um-buffer)
- [Associação entre Arquivo Físico e Arquivo Lógico](#associação-entre-arquivo-físico-e-arquivo-lógico)
  - [fopen](#fopen)
- [Fechamento de arquivos](#fechamento-de-arquivos)
  - [fclose](#fclose)
- [Leitura e Escrita](#leitura-e-escrita)
  - [fread](#fread)
  - [fwrite](#fwrite)
  - [fgets()](#fgets)
  - [gets()](#gets)
  - [sprintf()](#sprintf)
- [Funções de entrada (conio.h)](#funções-de-entrada-conioh)
  - [getch()](#getch)
  - [getche()](#getche)
- [Seeking (ponteiro do arquivo)](#seeking-ponteiro-do-arquivo)
  - [fseek](#fseek)
  - [ftell](#ftell)
  - [rewind](#rewind)
- [Limpeza de fluxo de saída ou entrada](#limpeza-de-fluxo-de-saída-ou-entrada)
  - [fflush](#fflush)
- [Tipos de organização de arquivos](#tipos-de-organização-de-arquivos)
  - [Sequência de Bytes (stream)](#sequência-de-bytes-stream)
  - [Organização em Campos](#organização-em-campos)
    - [Organização em Campos Fixos](#organização-em-campos-fixos)
    - [Indicador de comprimento](#indicador-de-comprimento)
    - [Campos separados por delimitadores](#campos-separados-por-delimitadores)
  - [Organização em Registros](#organização-em-registros)
    - [Registro de tamanho fixo](#registro-de-tamanho-fixo)
    - [Registro com tamanho fixo e campo variável](#registro-com-tamanho-fixo-e-campo-variável)
    - [Registro com indicador de tamanho](#registro-com-indicador-de-tamanho)
    - [Registro com indicador de tamanho e Token](#registro-com-indicador-de-tamanho-e-token)

<br><br>

# Conceitos iniciais

## O que é um buffer

Um buffer é uma área de memória usada para armazenar temporariamente dados enquanto eles estão sendo transferidos de um lugar para outro. Buffers são amplamente utilizados em programação para gerenciar a transferência de dados entre diferentes partes de um sistema, como entre um programa e um arquivo, entre diferentes componentes de software, ou entre hardware e software.

### Funcionamento de um buffer

Imagine que você está lendo dados de um arquivo e processando-os em seu programa. Se você ler cada byte individualmente, isso pode ser ineficiente, especialmente se a leitura de dados do disco (ou outra fonte) for lenta. Em vez disso, o sistema lê uma grande quantidade de dados de uma só vez e os armazena em um buffer. O programa pode então acessar esses dados diretamente do buffer, o que é muito mais rápido do que ler os dados byte a byte do disco.

# Associação entre Arquivo Físico e Arquivo Lógico

```c
FILE *p_arq;
if((p_arq = fopen("meu-arquivo.bat", "r")) == NULL )
    printf("Erro na abertura do Arquivo");
else
```

## fopen

**r**: abre para leitura; coloca o ponteiro do arquivo no começo do arquivo

**w**: abre somente para escrita; coloca o ponteiro do arquivo no começo do arquivo e reduz o comprimento do arquivo para zero. Se o arquivo não existir, tenta criá-lo.

**w+b**: Abre um arquivo binário para ler e escrever

# Fechamento de arquivos

## fclose

Encerra a associação entre arquivos lógico e físico,
garantindo que todas as informações sejam
atualizadas e salvas (conteúdo dos buffers de E/S
enviados para o arquivo).

```c
fd = fopen("meu-arquivo.bat", "r");
...
fclose(fd);
```

Em caso de êxito retorna 0, caso contrário retorna -1 (End of File)

# Leitura e Escrita

## fread

A função retorna o número de unidades
efetivamente lidas. Este número pode ser menor que
<nr> quando o fim do arquivo for encontrado ou
ocorrer algum erro (== 0).

```c
fread(&<dest-addr>, sizeof(<dest-addr>), <nr>, FILE* f)
```

`&<dest-addr>`: Endereço de um bloco de memória onde os dados lidos do arquivo serão armazenados

`sizeof(<dest-addr>)`: retorna o tamanho em bytes do bloco de memória apontado por `<dest-addr>`

`<nr>`: número de elementos a serem lidos

`FILE* f`: ponteiro para o arquivo no qual os dados seram lidos

## fwrite

É usada para escrever dados de um bloco de memória em um arquivo.

```c
fwrite(&<source-addr>, sizeof(<source-addr>), <nr>, FILE* f)
```

`&<source-addr>`: Endereço de memória de onde os dados serão escritos no arquivo

`sizeof(<source-add>)`: Tamanho, em bytes, de um único elemento do tipo de dados armazenado em `<source-addr>`.

`<nr>`: Número de elementos a serem escritos no arquivo

`FILE* f`: Ponteiro para o arquivo onde os dados serão escritos.

A função retorna o número de itens escritos. Este
valor será igual a `<nr>` a menos que ocorra algum
erro (== 0).

## fgets()

È usada para ler um único caractere de um arquivo em C.

É uma função simples e bastante eficiente para leitura de arquivos texto em pequenos trechos, mas para grandes volumes de dados, pode ser mais eficiente ler blocos maiores de dados (por exemplo, usando fread) e processá-los em memória.

```c
int fgetc(FILE *f);
```

A função retorna o próximo caractere do arquivo como um valor do tipo int, mas esse valor é realmente um caractere de 8 bits, que pode ser convertido para char.

Se a função atingir o fim do arquivo (EOF) ou ocorrer um erro, fgetc retorna a constante `EOF`, que é um valor especial definido em `<stdio.h>`.

## gets()

É usada para ler uma linha inteira de texto da entrada padrão (geralmente o teclado) até encontrar um caractere de nova linha (\n) ou o fim do arquivo (EOF).

```c
char *gets(char *str);
```

`char *str`: Um ponteiro para um buffer de caracteres onde a linha de entrada será armazenada. Este buffer deve ser grande o suficiente para conter a linha de texto lida, mais o caractere nulo \0 que marca o fim da string.

## sprintf()

É usada para formatar e armazenar uma string em um buffer. Ela funciona de forma semelhante à função `printf()`, mas em vez de enviar a saída para a tela (ou para a saída padrão), `sprintf()` grava a string formatada em um buffer de caracteres, que pode ser posteriormente manipulado como qualquer outra string em C.

```c
int sprintf(char *str, const char *format, ...);
```

`char *str`: Um ponteiro para o buffer de caracteres onde a string formatada será armazenada.

`const char *format`: Uma string de formato que especifica como a string resultante será formatada. Essa string pode conter texto literal e especificadores de formato, como `%d` para inteiros, `%s` para strings, `%f` para números de ponto flutuante, etc.

`...`: Uma lista de argumentos adicionais que correspondem aos especificadores de formato na string `format`. Estes argumentos são formatados e inseridos na string final.

# Funções de entrada (conio.h)

são amplamente utilizadas para capturar a entrada de caracteres diretamente do teclado sem a necessidade de pressionar Enter.

## getch()

Lê um único caractere do teclado, mas não exibe esse caractere na tela. Ou seja, a entrada é capturada "silenciosamente".

**Uso Comum**: É usada quando você quer capturar uma entrada do usuário sem exibi-la, como em casos de senha ou em menus interativos onde a exibição da tecla pressionada não é necessária.

## getche()

Lê um único caractere do teclado, mas exibe o caractere na tela enquanto o lê.

**Uso Comum**: É usada em situações onde você quer capturar a entrada do usuário e ao mesmo tempo exibi-la, como em prompts de entrada que precisam mostrar a tecla pressionada imediatamente.

# Seeking (ponteiro do arquivo)

## fseek

É usada para mover o "ponteiro do arquivo" para uma posição específica dentro de um arquivo. O ponteiro do arquivo é uma posição no arquivo onde a próxima operação de leitura ou escrita ocorrerá.

```c
int fseek(FILE *f, long int offset, int origem);
```

`FILE *f`: Este é o ponteiro para o arquivo que você quer manipular. Este ponteiro é obtido a partir da função fopen, que abre o arquivo.

`long int offset`: Este é o deslocamento, em bytes, que você deseja mover a partir da posição indicada por `origem`. Pode ser um valor positivo (para avançar no arquivo) ou negativo (para retroceder no arquivo).

`int origem`: Este parâmetro especifica a posição a partir da qual o deslocamento deve ser aplicado. Pode ter três valores possíveis:

| Nome     | Valor | Significado               |
| -------- | ----- | ------------------------- |
| SEEK_SET | 0     | Início do arquivo         |
| SEEK_CUR | 1     | Ponto corrente no arquivo |
| SEEK_END | 2     | Fim do arquivo            |

## ftell

Ela retorna a posição atual do ponteiro do arquivo, que pode ser útil para armazenar a posição atual antes de movê-lo com fseek, para que você possa voltar para essa posição depois.

```c
ftell(FILE *f)
```

## rewind

É usada para reposicionar o ponteiro do arquivo no início de um arquivo. Ela é uma função simples, mas muito útil, especialmente quando você precisa ler ou processar o mesmo arquivo várias vezes desde o começo.

```c
void rewind(FILE *f);
```

`FILE *f`: Um ponteiro para um arquivo aberto, que foi obtido através da função `fopen`.

# Limpeza de fluxo de saída ou entrada

## fflush

Em algumas implementações específicas, `fflush(stdin)` pode limpar o buffer de entrada, removendo qualquer dado pendente que ainda não foi processado.

```c
int fflush(stdin);
```

# Tipos de organização de arquivos

## Sequência de Bytes (stream)

A organização adotada para o arquivo é como uma sequência de caracteres.
Os dados são lidos do teclado e escritos num arquivo.
Uma vez escritas as informações, não existe como recuperar porções individuais (nome ou endereço).

Desta forma, perde-se a integridade das unidades fundamentais de organização dos dados:
* Os dados são agregados de caracteres com significado próprio;
* Tais agregados são chamados campos (fields). 

**Variáveis:**
```c
char nome[30], sobrenome[30], end[30];
FILE *out;
```

**Manipulação:**
```c
fwrite(nome,sizeof(char),strlen(nome),out);
fwrite(sobrenome,sizeof(char),strlen(sobrenome),out);
fwrite(end,sizeof(char),strlen(end),out);
```

**Diferença:** É escrito no arquivo o tamanho exato de cada variável de entrada dentro do binário

## Organização em Campos

Campo:
* menor unidade lógica de informação em um arquivo;
* uma noção lógica (ferramenta conceitual), não corresponde necessariamente a um conceito físico.

### Organização em Campos Fixos

**Variáveis:**
```c
char nome[30], sobrenome[30], end[30];
FILE *out;
```

**Manipulação:**

```c
fwrite(nome,sizeof(nome),1,out);
fwrite(sobrenome,sizeof(sobrenome),1,out);
fwrite(end,sizeof(end),1,out);
```

**Diferença:** É escrito no arquivo o tamanho total dos arrays, independente do tamanho da entrada

**Vantagem**
* facilidade na pesquisa.
* razoável se o comprimento dos campos é realmente fixo, ou apresenta pouca variação.

**Desvantagem**
* o espaço alocado (e não usado) aumenta desnecessariamente o tamanho do arquivo (desperdício).
* solução inapropriada quando se tem uma grande quantidade de dados com tamanho variável.
* dados podem precisar ser truncados.


### Indicador de comprimento

O tamanho de cada campo é armazenado imediatamente antes do dado.

**Variáveis:**
```c
char nome[30], sobrenome[30], end[30];
FILE *out;
int tam;
```

**Manipulação:**

```c
tam = strlen(nome);
tam++;
fwrite(&tam, sizeof(int), 1, out);
fwrite(nome, sizeof(char), tam, out);

tam = strlen(sobrenome);
tam++;
fwrite(&tam, sizeof(int), 1, out);
fwrite(sobrenome, sizeof(char), tam, out);

tam = strlen(end);
tam++;
fwrite(&tam, sizeof(int), 1, out);
fwrite(end, sizeof(char), tam, out);

//Leitura

rewind(out);
char buffer[255];
while (fread(&tam, sizeof(int), 1, out))
{
    fread(buffer, sizeof(char), tam, out);
    // buffer[tam]='\0';

    printf("\ncampo: %s", buffer);
    getch();
}

```

**Diferença:** Escrevemos na frente de toda string do arquivo o tamanho inteiro da variável seguinte

**Vantagens**

* Economia de espaço de armazenamento, mesmo com a necessidade de se gastar alguns bytes adicionais para guardar o tamanho dos campos.
* Dados não precisam ser truncados.

**Desvantagens**

* Dificuldade na pesquisa

### Campos separados por delimitadores

**Variáveis:**

```c
char nome[30], sobrenome[30], end[30], campo[30];
char buffer[100];
FILE *out;
int tam_campo;
```

**Manipulação:** 

```c
sprintf(buffer,"%s|%s|%s|",nome,sobrenome,end);
fwrite(buffer,sizeof(char),strlen(buffer),out);

//Leitura

fseek(out,0,0);
tam_campo = pega_campo(out,campo);	 
while (tam_campo > 0)
  {        
      printf("tamanho: %d\n",tam_campo);
      printf("campo: %s\n\n",campo);  
      tam_campo = pega_campo(out,campo);        
      getch();      
  }

int pega_campo(FILE *p_out, char *p_campo)
{
     char ch;
     int i=0;

     p_campo[i] = '\0';
     
     /*ch = fgetc(p_out);     
     while ((ch != '|') && (ch != EOF))
      {                      
           p_campo[i] = ch;
           i++;
           ch = fgetc(p_out);           
      }     */
      
     while (fread(&ch,sizeof(char),1,p_out))
       {
           if (ch == '|')
             break;
           else p_campo[i] = ch;
           
           i++;
       }
     
     p_campo[i] = '\0';
     
     return strlen(p_campo);
} 
```

**Diferença:** É escrito no arquivo o tamanho total de entrada do usuário, delimitando os campos com '|'. É feito também uma função `pega_campo` para ler o tamanho de cada campo.

**Vantagem:** 

* economia de espaço de armazenamento.

**Desvantagem:** 

* dificuldade na pesquisa.
* necessidade de escolha de um delimitador que não pertence ao domínio dos dados.

## Organização em Registros

É um outro nível de organização imposto aos dados com o objetivo de preservar o significado.

### Registro de tamanho fixo

Analogamente ao conceito de campos de tamanho fixo, assume que todos os registros têm o mesmo número de bytes.

**Variáveis:**
```c
struct estrutura
{
  char nome[30], sobrenome[30], end[30];
} ts;

FILE *out;
```

**Manipulação:**
```c
fwrite(&ts, sizeof(ts), 1, out);

//Leitura

while (fread(&ts, sizeof(ts), 1, out))
{
  printf("\nnome: %s", ts.nome);
  printf("\nsobrenome: %s", ts.sobrenome);
  printf("\nendereco: %s", ts.end);
  getch();
}
```

**Diferença:** Agora estamos trabalhando com registro, não há delimitador entre eles e todas as strings de entradas são armazenadas no arquivo com o tamanho de 30.

**Vantagens**
* Eficiência de armazenamento 
* acesso direto aos registros.

**Desvantagens**
* rigidez na estrutura dos registros.

### Registro com tamanho fixo e campo variável

* Ao invés de especificar que cada registro contém um número fixo de bytes, podemos especificar um número fixo de campos.

* O tamanho do registro, em bytes, é variável.

* Neste caso, os campos seriam separados por delimitadores.

**Variáveis:**
```c
char nome[30], sobrenome[30], end[30];
char registro[90];
FILE *out;
```

**Manipulação:**
```c
sprintf(registro,"%s|%s|%s|",nome,sobrenome,end);
fwrite(registro,sizeof(registro),1,out);  
```

**Diferença:** Registro age como um buffer, assim colocamos nome, sobrenome e endereço dentro do registro com delimitadores

**Vantagens**
* Flexibilidade nos Campos
* Facilidade de Leitura/Escrita
* Acesso Direto ao Registro

**Desvantagens**

* Desperdício de Espaço
* Rigidez no Tamanho do Registro
* Dificuldade de Manipulação

### Registro com indicador de tamanho

**Variáveis:**
```c
char nome[30], end[30], campo[30], registro[90];
FILE *out;
int ra, tam_campo, tam_reg, pos;
```

**Manipulação:**
```c
sprintf(registro,"%d|%s|%s|",ra,nome,end);
tam_reg = strlen(registro);
fwrite(&tam_reg, sizeof(int), 1, out);
fwrite(registro, sizeof(char), tam_reg, out);

//Leitura

fseek(out, 0, 0);
  tam_reg = pega_registro(out, registro);
  while (tam_reg > 0)
  {
    pos = 0;
    tam_campo = pega_campo(registro, &pos, campo);
    while (pos <= tam_reg)
    {
      printf("tamanho: %d\n", tam_campo);
      printf("campo: %s\n\n", campo);
      tam_campo = pega_campo(registro, &pos, campo);
    }
    tam_reg = pega_registro(out, registro);
    getch();
  }

  fclose(out);

  getch();
}

int pega_registro(FILE *p_out, char *p_reg)
{
  int bytes;

  //Tenta ler um inteiro
  if (!fread(&bytes, sizeof(int), 1, p_out))
    return 0;
  else
  {
    fread(p_reg, bytes, 1, p_out);
    return bytes;
  }
}

int pega_campo(char *p_registro, int *p_pos, char *p_campo)
{
  char ch;
  int i = 0;

  p_campo[i] = '\0';

  ch = p_registro[*p_pos];
  // while ((ch != '|') && (ch!=EOF))//estava assim
  while ((ch != '|') && (ch != '\0'))
  {
    p_campo[i] = ch;
    i++;
    ch = p_registro[++(*p_pos)];
  }
  ++(*p_pos);
  p_campo[i] = '\0';

  return strlen(p_campo);
}

```

**Diferença:** Aqui, além de usarmos delimitadores, precisamos lidar com registros e campos para lermos o arquivo. A função pega_registro tem o objetivo de encontrar um inteiro (tamanho do registro) para saber o tamanho do registro. A função pega_campo tem o objetivo de retornar o tamanho do campo e guardar a posição do último campo percorrido.

**Vantagens**

* Eficiência no Uso de Espaço
* Flexibilidade
* Acesso Direto e Processamento Simples
* Possibilidade de Alterações

**Desvantagens**

* Complexidade Adicional
* Fragmentação
* Leitura Sequencial Necessária para Acesso aos Campos
* Dependência do Delimitador

### Registro com indicador de tamanho e Token

**Variáveis:**
```c
char nome[30], end[30], registro[90];
char teste[30];
FILE *out;
int ra, tam_reg;
char *pch;
```

**Manipulação:**
```c
sprintf(registro,"%d|%s|%s|",ra,nome,end);
tam_reg = strlen(registro);
tam_reg++;
fwrite(&tam_reg, sizeof(int), 1, out);
fwrite(registro, sizeof(char), tam_reg, out);

//Leitura

  fseek(out, 0, 0);
  tam_reg = pega_registro(out, registro);
  while (tam_reg > 0)
  {
    pch = strtok(registro, "|");
    while (pch != NULL)
    {
      printf("%s\n", pch);
      // strcpy(teste,pch);
      // printf("%s\n",teste);
      pch = strtok(NULL, "|");
    }
    printf("\n");
    tam_reg = pega_registro(out, registro);
    getch();
  }

int pega_registro(FILE *p_out, char *p_reg)
{
  int bytes;

  if (!fread(&bytes, sizeof(int), 1, p_out))
    return 0;
  else
  {
    fread(p_reg, bytes, 1, p_out);
    return bytes;
  }
}

```

**Diferença:** É semelhante a manipulação anterior, contudo agora usamos uma função em C da biblioteca <string.h> que já faz a função do pega_campo automaticamente. A função usada é a `strtok(registro, "|");`, em que ela quebra a string dado um separador. Importante ressaltar que ela retorna um endereço onde ocorre a primeira ocorrência de `|` e substitui ele por `NULL`. Em `strtok(NULL, "|");` ele busca a próxima ocorrência do pipeline após `NULL`.

**Vantagens**

* Eficiência no Armazenamento
* Facilidade de Leitura e Escrita
* Flexibilidade
* Processamento Simples

**Desvantagens**

* Complexidade na Modificação
* Potencial para Erros
* Performance
* Fragmentação de Arquivos:

