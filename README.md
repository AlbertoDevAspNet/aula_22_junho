# aula_22_junho

# Criar uma branch de feature
git checkout develop
git checkout -b feature/login-page

# Trabalhar na feature...
git add .
git commit -m "feat(auth): implementa tela de login"

# Finalizar a feature (merge na develop)
git checkout develop
git merge feature/login-page
git branch -d feature/login-page

# Criar uma release
git checkout -b release/1.0.0
# Ajustes finais, bump de versão...
git checkout main
git merge release/1.0.0
git tag -a v1.0.0 -m "Release 1.0.0"
git checkout develop
git merge release/1.0.0


# Parte pratica 

# Criar branch a partir da main
git checkout main
git pull origin main
git checkout -b add-dark-mode

# Fazer suas alterações e commits
git add .
git commit -m "feat(ui): adiciona modo escuro"

# Push e abrir Pull Request
git push origin add-dark-mode
# → Abre PR no GitHub
# → CI roda testes automaticamente
# → Code review
# → Merge = Deploy automático 🚀


# .github/workflows/ci.yml
name: CI Pipeline

# Quando executar
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [18.x, 20.x]
    
    steps:
      # 1. Checkout do código
      - name: Checkout repository
        uses: actions/checkout@v4

      # 2. Configurar Node.js
      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      # 3. Instalar dependências
      - name: Install dependencies
        run: npm ci

      # 4. Rodar linter
      - name: Run linter
        run: npm run lint

      # 5. Rodar testes
      - name: Run tests
        run: npm test

      # 6. Build
      - name: Build project
        run: npm run build

        
