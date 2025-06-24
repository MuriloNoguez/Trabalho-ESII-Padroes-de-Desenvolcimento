# 🎯 Padrão de Projeto: Singleton

## 📘 O que são Padrões de Projeto?
Padrões de projeto são soluções reutilizáveis para problemas recorrentes durante o desenvolvimento de software orientado a objetos. Eles ajudam a tornar o código mais organizado, manutenível e escalável.

---

## 🔷 Singleton

### ✔️ Definição
O padrão Singleton garante que uma classe tenha apenas **uma instância** e fornece um ponto de acesso global a essa instância.

### 💡 Quando Usar?
- Quando precisa garantir que só exista **uma instância** de uma classe em toda a aplicação;
- Quando você quer controlar o acesso a um recurso compartilhado (ex: banco de dados, logger, configurações globais).

---

## 🚫 Exemplo **Sem** Singleton (TypeScript)

.ts
// loggerSemSingleton.ts

class Logger {
  log(message: string): void {
    console.log(`[LOG]: ${message}`);
  }
}

const logger1 = new Logger();
const logger2 = new Logger();

logger1.log("Primeira instância");
logger2.log("Segunda instância");

console.log(logger1 === logger2); // false

## 🚫 Exemplo **Com** Singleton (TypeScript)

.ts
// loggerComSingleton.ts

class SingletonLogger {
  private static instance: SingletonLogger;

  private constructor() {} // impede uso de new fora da classe

  static getInstance(): SingletonLogger {
    if (!SingletonLogger.instance) {
      SingletonLogger.instance = new SingletonLogger();
    }
    return SingletonLogger.instance;
  }

  log(message: string): void {
    console.log(`[LOG]: ${message}`);
  }
}

// Testando
const loggerA = SingletonLogger.getInstance();
const loggerB = SingletonLogger.getInstance();

loggerA.log("Usando instância A");
loggerB.log("Usando instância B");

console.log(loggerA === loggerB); // true

## ⚖️ Pontos Fortes
- Garante que apenas uma instância exista;
- Controla o acesso a recursos compartilhados;
- Fácil de implementar;

## ❌ Pontos Fracos
- Viola o princípio da responsabilidade única (SRP);
- Dificulta testes unitários (difícil de mockar);
- Pode gerar acoplamento global se mal utilizado;

## ✅ Conclusão
O padrão Singleton é bastante útil em casos onde apenas uma instância de uma classe deve existir, como loggers, configurações globais, gerenciadores de conexão, entre outros.

Apesar de ser simples e eficaz, deve ser usado com cautela para evitar acoplamento excessivo e dificuldade na testabilidade do sistema.

# 🏭 Padrão de projeto: Factory Method

## 🔷 O que é?

O **Factory Method** é um padrão de projeto **criacional** que fornece uma interface para criar objetos, mas permite que as subclasses decidam **qual classe concreta será instanciada**. Ele ajuda a **desacoplar o código** que usa o objeto do código que cria o objeto.

---

## 💡 Quando Usar?

- Quando você quer evitar acoplamento direto com classes concretas;
- Quando seu sistema precisa ser facilmente **extensível**, adicionando novos tipos sem mudar o código existente;
- Quando a criação dos objetos envolve lógica complexa ou variações de um mesmo tipo.

---

## ❌ Exemplo Sem Factory Method (TypeScript)

//ts
class EmailNotificacao {
  enviar(msg: string) {
    console.log(`[Email] ${msg}`);
  }
}

class SMSNotificacao {
  enviar(msg: string) {
    console.log(`[SMS] ${msg}`);
  }
}

// Código cliente decide qual classe usar
const tipo = "sms";
let notificacao;

if (tipo === "email") {
  notificacao = new EmailNotificacao();
} else if (tipo === "sms") {
  notificacao = new SMSNotificacao();
}

notificacao.enviar("Você tem uma nova mensagem.");

## 🔴 Problemas:

- O cliente está fortemente acoplado às classes concretas;

- Toda mudança de tipo exige alteração no código cliente;

- Difícil de escalar e manter.

//ts

interface Notificacao {
  enviar(msg: string): void;
}

class EmailNotificacao implements Notificacao {
  enviar(msg: string) {
    console.log(`[Email] ${msg}`);
  }
}

class SMSNotificacao implements Notificacao {
  enviar(msg: string) {
    console.log(`[SMS] ${msg}`);
  }
}

abstract class NotificacaoFactory {
  abstract criar(): Notificacao;
}

class EmailFactory extends NotificacaoFactory {
  criar(): Notificacao {
    return new EmailNotificacao();
  }
}

class SMSFactory extends NotificacaoFactory {
  criar(): Notificacao {
    return new SMSNotificacao();
  }
}

// Código cliente desacoplado
const factory: NotificacaoFactory = new SMSFactory();
const notificacao = factory.criar();
notificacao.enviar("Você tem uma nova mensagem.");

## 🟢 Benefícios:

O cliente não sabe nem se preocupa com o tipo concreto;

Código mais limpo, modular e preparado para crescer;

Novos tipos podem ser adicionados com facilidade (ex: WhatsApp, Telegram, etc).

## ⚖️ Pontos Fortes
✔️ Reduz o acoplamento entre o código que usa e o que cria os objetos;
✔️ Torna o sistema mais extensível e de fácil manutenção;
✔️ Segue os princípios SOLID (especialmente o OCP - Aberto para extensão, fechado para modificação).

❌ Pontos Fracos
✖️ Adiciona mais classes e estrutura ao sistema;
✖️ Pode parecer complexo demais para sistemas pequenos ou simples.

## ✅ Conclusão
O Factory Method é uma ótima solução quando precisamos criar objetos de maneira controlada e flexível, especialmente em sistemas que precisam crescer e se adaptar com o tempo.

Embora pareça mais "verbooso" que o uso direto de new, ele oferece muito mais poder de organização, manutenção e testabilidade, e evita que o cliente precise conhecer todos os tipos concretos do sistema.

## 🔍 Comparativo: Singleton vs Factory Method

| Aspecto                     | Singleton                                            | Factory Method                                                        |
|----------------------------|------------------------------------------------------|------------------------------------------------------------------------|
| 🎯 **Objetivo**            | Garantir que uma classe tenha **uma única instância**| Delegar a criação de objetos para subclasses                          |
| 🏗️ **Categoria**           | Criacional                                           | Criacional                                                             |
| 🧠 **Controle de instância**| Total: apenas uma instância é criada                 | Nenhum: múltiplas instâncias podem ser criadas conforme a necessidade |
| 🔄 **Flexibilidade**        | Baixa: restringe a criação de múltiplas instâncias   | Alta: permite escolher dinamicamente qual classe instanciar           |
| 🔌 **Desacoplamento**       | Baixo: código depende diretamente da classe Singleton| Alto: o cliente depende de uma interface, não da implementação        |
| 🧪 **Testabilidade**        | Difícil de testar (instância global é difícil de mockar)| Fácil de testar (objetos podem ser mockados via interface)           |
| 📦 **Exemplo típico**       | Logger, Configuração global, Conexão de BD           | Criador de notificações, documentos, interfaces diferentes            |
| 💬 **Instância única**      | Sim                                                  | Não                                                                   |
| 🧱 **Complexidade estrutural** | Simples e direta                                  | Envolve hierarquia de classes (fábricas e produtos)                  |
| ✅ **Prós**                 | Controle total da instância, simples de usar         | Altamente extensível e desacoplado                                   |
| ❌ **Contras**              | Acoplamento global, difícil de testar, quebra SRP    | Estrutura mais complexa, pode ser “overkill” para casos simples       |

---

## 🎯 Conclusão do Comparativo

- O **Singleton** é ideal quando se quer **garantir uma única instância** de algo no sistema e esse algo precisa estar acessível globalmente. É simples e direto, mas pode trazer problemas de acoplamento e dificultar testes.

- Já o **Factory Method** brilha quando queremos **desacoplar a criação de objetos do uso desses objetos**, tornando o sistema mais extensível e aderente aos princípios do SOLID, especialmente o OCP (Aberto para extensão, fechado para modificação).

👉 **Resumo prático**:  
Use **Singleton** quando quiser **controlar o número de instâncias**.  
Use **Factory Method** quando quiser **flexibilidade na criação de objetos sem depender das classes concretas**.
