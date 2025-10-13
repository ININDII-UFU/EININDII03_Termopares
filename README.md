# Trabalho final ININ2 — Markdown extraído

> Este arquivo foi gerado automaticamente a partir do DOCX original (`Trabalho final ININ2.docx`).

## Conteúdo (texto extraído)

Instrumentação Industrial II



Trabalho Final – Malha de Tiragem

Professor: Josué Silva de Morais









































Nomes:

Keslley Brito Ramos 	11921EAU002

Luiz Felipe Carneiro De Oliveira	12211EAU022







Uberlândia (MG)

30/09/2025

Acesso e Estrutura da Malha de Tiragem

Para iniciar o desenvolvimento, acesse o projeto previamente disponibilizado. A malha de controle de tiragem está localizada no caminho: Lógica > Application > TIRAGEM, conforme destacado na interface do CODESYS.







O desenvolvimento desta malha de controle exigirá a utilização de três blocos funcionais distintos:

LIN_TRAFO: Para a transformação linear de sinais.

PID: O controlador principal da malha.

REAL_TO_WORD: Para a conversão de tipos de dados, necessária para a comunicação.











Importação de Bibliotecas e Blocos Funcionais



Os blocos LIN_TRAFO e PID não são componentes nativos do ambiente CODESYS e precisam ser importados por meio de uma biblioteca externa.

Para realizar a importação, siga os passos abaixo:

No navegador do projeto, acesse o Gestor de biblioteca dentro da Application.

Clique em Adicionar biblioteca.







Pesquise pela biblioteca "Util".







Dentro da biblioteca "Util", o bloco LIN_TRAFO pode ser encontrado na pasta "Mathematical Functions", e o bloco PID está localizado na pasta "Controller". 













O bloco REAL_TO_WORD, por sua vez, é um componente nativo e pode ser encontrado na Caixa de ferramentas, na seção Conversão. 







Configuração das Variáveis de Controle



As principais variáveis utilizadas nesta malha de controle são: 

FIT100AR: Representa a vazão de ar. 

FV100AR: Corresponde à válvula que controla a vazão de ar.







A configuração dessas variáveis envolve associá-las aos respectivos terminais dos blocos. Para variáveis provenientes da comunicação Modbus, siga este procedimento:



Clique nas reticências (...) do campo de entrada do bloco onde a variável será utilizada.







Na janela do Assistente de entrada, navegue até IoConfig_Globals_Mapping, onde as variáveis de comunicação Modbus estão listadas.







Selecione a variável desejada. Por exemplo, para a leitura do sensor de vazão, utilize a tag R_FIT100AR. 







Desenvolvimento da Lógica de Controle da Malha de Tiragem

A construção da malha de tiragem é realizada em etapas sequenciais, configurando cada bloco para sua função específica.

Bloco 1: Conversão do Sinal do Sensor (LIN_TRAFO)

A primeira etapa consiste em ler e normalizar o sinal do sensor de vazão de ar (FIT100AR). O valor proveniente da comunicação Modbus é um inteiro de 2 bytes, com uma faixa de 0 a 65535. Para que esse valor seja compatível com o bloco PID, ele deve ser normalizado para uma escala de 0 a 600, utilizando o bloco LIN_TRAFO. 

Entrada (IN): R_FIT100AR (variável do Modbus). 

Entrada (IN_MIN): 0 

Entrada (IN_MAX): 65535 

Saída (OUT_MIN): 0 

Saída (OUT_MAX): 600 







Ao inserir o bloco, ele deve ser instanciado com um nome único, por exemplo, LIN_TRAFO_R_FIT100AR.







A saída normalizada deste bloco (LIN_TRAFO_R_FIT100AR.OUT) será conectada à entrada ACTUAL do controlador PID.



Bloco 2: Controlador PID

O bloco PID é o núcleo da malha de controle. Sua configuração é a seguinte:

Entrada (ACTUAL): Conectada à saída do primeiro LIN_TRAFO (LIN_TRAFO_R_FIT100AR.OUT), que representa o valor atual da vazão de ar.







Entrada (SET_POINT): Associada à variável SP_TIRAGEM, que define o valor desejado para a vazão. Esta variável deve ser declarada como do tipo REAL.











Parâmetros de Sintonia (KP, TN, TV): Inserir os valores de ganho Proporcional (KP), Integral (KI/TN) e Derivativo (KD/TV) obtidos previamente. Neste caso, KP = 14.8425 e TN = 21.185.







Entrada Manual (Y_MANUAL): Conectada à variável R_FV100AR, que reflete o valor de abertura da válvula quando em modo manual.















Limites de Saída (Y_MIN, Y_MAX): Definidos como 0 e 100, respectivamente.

Offset (Y_OFFSET): Configurado como 0.







Seleção de Modo (MANUAL): Conectada à variável FV100AR_AM, que determina se o controle é automático ou manual.











Reset (RESET): Configurado com o valor booleano FALSE.







O bloco de PID deve, por fim, ficar da seguinte forma:







Este bloco também deve ser instanciado com um nome, como PID_TIRAGEM.







Bloco 3: Conversão do Sinal de Saída (LIN_TRAFO)



A saída do controlador PID (PID_TIRAGEM.Y), que opera em uma faixa de 0 a 100, precisa ser convertida de volta para o padrão da comunicação Modbus (0 a 65535) antes de ser enviada para o atuador (válvula FV100AR). Um segundo bloco LIN_TRAFO é utilizado para esta finalidade.

Entrada (IN): PID_TIRAGEM.Y (saída do controlador PID). 

Entrada (IN_MIN): 0 

Entrada (IN_MAX): 100 

Saída (OUT_MIN): 0 

Saída (OUT_MAX): 65535







Bloco 4: Conversão de Tipo de Dado (REAL_TO_WORD)



Finalmente, como a comunicação Modbus requer um formato de dados do tipo WORD, é necessário converter a saída do último bloco LIN_TRAFO (que é do tipo REAL) para WORD. 

Entrada: LIN_TRAFO_W_FV100AR.OUT (saída do bloco de conversão anterior). 

Saída: Conectada à variável do registro Modbus responsável por escrever na válvula, W_FV100AR.

## Figuras (extraídas do DOCX)

**Figura 1.**
![Figura 1](figuras/image1.png)

**Figura 2.**
![Figura 2](figuras/image10.png)

**Figura 3.**
![Figura 3](figuras/image11.png)

**Figura 4.**
![Figura 4](figuras/image12.png)

**Figura 5.**
![Figura 5](figuras/image13.png)

**Figura 6.**
![Figura 6](figuras/image14.png)

**Figura 7.**
![Figura 7](figuras/image15.png)

**Figura 8.**
![Figura 8](figuras/image16.png)

**Figura 9.**
![Figura 9](figuras/image17.png)

**Figura 10.**
![Figura 10](figuras/image18.png)

**Figura 11.**
![Figura 11](figuras/image19.png)

**Figura 12.**
![Figura 12](figuras/image2.png)

**Figura 13.**
![Figura 13](figuras/image20.png)

**Figura 14.**
![Figura 14](figuras/image21.png)

**Figura 15.**
![Figura 15](figuras/image22.png)

**Figura 16.**
![Figura 16](figuras/image23.png)

**Figura 17.**
![Figura 17](figuras/image24.png)

**Figura 18.**
![Figura 18](figuras/image25.png)

**Figura 19.**
![Figura 19](figuras/image26.png)

**Figura 20.**
![Figura 20](figuras/image27.png)

**Figura 21.**
![Figura 21](figuras/image28.png)

**Figura 22.**
![Figura 22](figuras/image29.png)

**Figura 23.**
![Figura 23](figuras/image3.png)

**Figura 24.**
![Figura 24](figuras/image30.png)

**Figura 25.**
![Figura 25](figuras/image4.png)

**Figura 26.**
![Figura 26](figuras/image5.png)

**Figura 27.**
![Figura 27](figuras/image6.png)

**Figura 28.**
![Figura 28](figuras/image7.png)

**Figura 29.**
![Figura 29](figuras/image8.png)

**Figura 30.**
![Figura 30](figuras/image9.png)
