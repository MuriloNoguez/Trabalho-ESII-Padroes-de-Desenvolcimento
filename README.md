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
