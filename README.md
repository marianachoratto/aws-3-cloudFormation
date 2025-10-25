# Desafio 3- Notas sobre o CloudFormation

Este repositório é destinado para guardar minhas anotações sobre o serviço AWS CloudFormation. Ele é um entregável do bootcamp **Santander Code Girl 2025: AWS**
# Descomplicando o AWS CloudFormation

Imagine que você é um chef de cozinha e precisa preparar um prato complexo, com 50 ingredientes e 30 passos. Agora, imagine que você precisa preparar exatamente esse mesmo prato, 100 vezes por dia, sem errar nenhum ingrediente ou passo.

Fazer isso manualmente seria um pesadelo, certo? Você poderia esquecer um ingrediente, errar a ordem dos passos ou a temperatura do forno.

O que você faria? Você criaria uma receita mestre. O AWS CloudFormation é exatamente isso: a sua "receita mestre" para a infraestrutura na nuvem.

O AWS CloudFormation é um processo que auxilia na automação de criação de recursos na AWS por meio de templates JSON ou YAML.

## 1\. O que é o AWS CloudFormation?

O CloudFormation é o serviço de Infraestrutura como Código (IaC) da AWS.

**"Infraestrutura"**: São os recursos que você usa na AWS. Pense em servidores (instâncias EC2), bancos de dados (RDS), redes (VPC), buckets de armazenamento (S3), etc.

**"como Código"**: Em vez de clicar em dezenas de botões no console da AWS para criar esses recursos (o "modo manual"), você escreve um arquivo de texto simples que descreve todos os recursos que você quer.

Esse arquivo de texto é a sua "receita".

## 2\. Os Componentes Principais 

Existem dois conceitos que você precisa saber para a prova:

**Template (O Modelo):**

  * **O que é:** É o seu arquivo de texto (escrito em formato YAML ou JSON).
  * **Analogia:** Esta é a sua Receita Mestre. Nela, você lista todos os "ingredientes" que você precisa. (Ex: `AWS::EC2::Instance` ou `AWS::S3::Bucket`) e as "instruções" (como eles devem ser configurados e conectados entre si).
  * **Exemplo:** Seu template pode dizer: "Eu quero 1 servidor EC2 do tipo t2.micro, 1 banco de dados RDS e 1 grupo de segurança que permita ao servidor falar com o banco de dados."

**Stack (A Pilha):**

  * **O que é:** É o conjunto de recursos reais que foram criados na AWS a partir do seu template.
  * Quando você entrega sua receita (Template) ao CloudFormation e manda "executar", ele lê as instruções e "cozinha" a infraestrutura para você. O resultado final, todos os recursos funcionando juntos, é o seu Stack.
  * **Ponto-chave:** O Stack é uma unidade. Você gerencia todos aqueles recursos (servidor, banco de dados, etc.) como um grupo único. Se você quiser atualizar algo, você atualiza a "receita" (Template) e manda o CloudFormation "re-cozinhar" (atualizar o Stack). Se quiser apagar tudo, você simplesmente "joga o prato fora" (exclui o Stack), e o CloudFormation remove todos os recursos associados.

## 3\. Por que usar o CloudFormation? Os Benefícios

Isto é muito importante para a prova CLF-C02. Por que não simplesmente clicar no console?

  * **Automatização e Padronização:**
      * *A Solução (IaC):* Com um template, a infraestrutura é criada da mesma forma todas as vezes. Você define o padrão uma vez (na "receita") e a AWS o executa perfeitamente sempre.
  * **Repetibilidade:**
      * *O Cenário:* Você criou uma infraestrutura completa para sua aplicação em "Desenvolvimento". Agora, você precisa de um ambiente idêntico para "Testes" e outro para "Produção". Em vez de passar dias clicando e tentando lembrar cada passo, você simplesmente pega o mesmo template (a "receita") e cria um novo Stack. Em minutos, você tem um clone perfeito do seu ambiente.
  * **Gerenciamento do Ciclo de Vida:**
      * O CloudFormation não serve apenas para criar. Ele também serve para atualizar e excluir.
      * *Atualizar:* Se você precisar trocar seu servidor de `t2.micro` para `t3.small`, você não cria um novo. Você edita o template (a "receita") e pede ao CloudFormation para "Atualizar o Stack". Ele entende o que mudou e aplica apenas aquela mudança.
      * *Excluir:* Quando o projeto acabar, você manda "Excluir o Stack". O CloudFormation rastreia tudo o que ele criou e apaga tudo para você, garantindo que você não deixe recursos "órfãos" ligados, gastando dinheiro.
  * **Infraestrutura é Código (Revisão e Versionamento):**
      * Como o template é apenas um arquivo de texto, você pode tratá-lo como código de software.
      * Você pode salvá-lo no Git, ver quem fez qual mudança (versionamento), pedir para colegas revisarem (code review) e testá-lo antes de aplicar em produção.

### Exemplo de JSON: criando um bucket S3

```json
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "Um template simples para criar um bucket S3 para estudos da CLF-C02.",
  "Resources": {
    "MeuBucketDeEstudoS3": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "Tags": [
          {
            "Key": "Ambiente",
            "Value": "Estudo-CLF"
          }
        ]
      }
    }
  }
}
```

### Entendendo este "Receita" (O Template)

Vamos quebrar o que cada parte desse JSON significa, de forma bem didática:

**"Description": "Um template simples..."**
É um comentário para humanos. Ajuda você e sua equipe a entender rapidamente o que este template faz.


**"Resources": { ... }**
*Tradução: "Aqui estão os 'Ingredientes' que eu quero que você prepare."*


> 💡
> Esta é a seção mais importante do template. É aqui que você lista todos os recursos da AWS que você quer criar.

**"MeuBucketDeEstudoS3": { ... }**
*Tradução: "O primeiro ingrediente que eu quero tem o apelido de 'MeuBucketDeEstudoS3'."*
Este é o ID Lógico. É apenas um "apelido" que você inventa para se referir a este recurso dentro do template. Não é o nome real do bucket.


**"Type": "AWS::S3::Bucket"**
*Tradução: "O tipo de ingrediente é: um Bucket S3."*
Isso diz ao CloudFormation exatamente qual serviço da AWS você quer usar. O formato é sempre `AWS::Serviço::Recurso`.


**"Properties": { ... }**
*Tradução: "Aqui estão as 'Instruções de Preparo' para este ingrediente."*
É aqui que você define as configurações do seu bucket.


**"Tags": [ ... ]**
*Tradução: "Coloque uma etiqueta (Tag) neste bucket."*
Tags são fundamentais para a organização, especialmente para rastrear custos (um tópico quente na CLF-C02\!). Aqui, estamos adicionando uma tag chamada `Ambiente` com o valor `Estudo-CLF`.
