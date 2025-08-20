# Resolvendo Conflitos Git - Guia Prático

## Cenário: Merge com Conflitos

### 1. Situação Inicial - ANTES

Quando você tenta fazer merge e há conflitos, o Git mostra uma mensagem como esta:

```bash
$ git merge feature-branch
Auto-merging src/main.py
CONFLICT (content): Merge conflict in src/main.py
Automatic merge failed; fix conflicts and then commit the result.
```

**Status do repositório:**
```bash
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   src/main.py

no changes added to commit (use "git add" or "git commit -a")
```

### 2. Identificando o Conflito

O arquivo `src/main.py` contém marcadores de conflito:

```python
def calculate_price(base_price, discount):
    """Calcula o preço com desconto"""
<<<<<<< HEAD
    # Versão da branch main
    if discount > 0.5:
        discount = 0.5  # Limite máximo de 50%
    return base_price * (1 - discount)
=======
    # Versão da branch feature
    if discount > 0.3:
        discount = 0.3  # Limite máximo de 30%
    final_price = base_price - (base_price * discount)
    return final_price
>>>>>>> feature-branch
```

**Explicação dos marcadores:**
- `<<<<<<< HEAD`: Início da versão atual (branch main)
- `=======`: Separador entre as versões
- `>>>>>>> feature-branch`: Fim da versão que está sendo mergeada

### 3. Resolvendo o Conflito - DEPOIS

#### Opção 1: Resolução Manual

Editamos o arquivo removendo os marcadores e combinando o melhor das duas versões:

```python
def calculate_price(base_price, discount):
    """Calcula o preço com desconto"""
    # Implementação que combina as duas abordagens
    if discount > 0.4:  # Compromisso entre 30% e 50%
        discount = 0.4  # Limite máximo de 40%
    
    final_price = base_price * (1 - discount)
    return final_price
```

#### Opção 2: Usando Ferramentas Visuais

```bash
# Configurar ferramenta de merge (uma vez)
$ git config --global merge.tool vimdiff

# Abrir ferramenta visual para resolver conflito
$ git mergetool src/main.py
```

### 4. Finalizando a Resolução

Após resolver todos os conflitos:

```bash
# Adicionar arquivo resolvido
$ git add src/main.py

# Verificar status
$ git status
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   src/main.py

# Finalizar o merge
$ git commit
```

O Git abrirá um editor com uma mensagem padrão:
```
Merge branch 'feature-branch'

# Conflicts:
#       src/main.py
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.
```

### 5. Resultado Final

```bash
$ git log --oneline --graph -5
*   a1b2c3d (HEAD -> main) Merge branch 'feature-branch'
|\  
| * d4e5f6g (feature-branch) Add discount validation
* | g7h8i9j Update price calculation logic
|/  
* j0k1l2m Initial price calculation function
```

## Comandos Úteis para Conflitos

### Visualizar Conflitos
```bash
# Ver arquivos com conflito
$ git status

# Ver diferenças detalhadas
$ git diff

# Ver conflitos de forma mais clara
$ git diff --name-only --diff-filter=U
```

### Opções Durante o Merge
```bash
# Aceitar versão atual (HEAD)
$ git checkout --ours <arquivo>

# Aceitar versão que está sendo mergeada
$ git checkout --theirs <arquivo>

# Abortar o merge completamente
$ git merge --abort

# Continuar após resolver conflitos
$ git commit
```

### Prevenção de Conflitos
```bash
# Sincronizar antes de trabalhar
$ git pull origin main

# Fazer rebase em vez de merge
$ git rebase main

# Fazer commits pequenos e frequentes
$ git commit -m "Small incremental change"
```

## Dicas Importantes

1. **Sempre teste** o código após resolver conflitos
2. **Comunique-se** com a equipe sobre mudanças importantes
3. **Use branches** para features isoladas
4. **Faça backup** antes de resolver conflitos complexos
5. **Configure** uma boa ferramenta de merge visual

## Ferramentas Recomendadas

- **VS Code**: Extensão GitLens
- **Terminal**: `vimdiff`, `meld`
- **GUI**: GitKraken, Sourcetree
- **IDE**: IntelliJ IDEA, PyCharm

---

*Este guia serve como referência rápida para resolução de conflitos Git. Adapte os exemplos conforme sua linguagem e projeto específico.*