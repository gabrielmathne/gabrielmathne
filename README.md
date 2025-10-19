
</p>

<h1 align="center">👋 Olá, eu sou o Gabriel Mathne!</h1>

<p align="center">
  💻 Estudante de <b>Engenharia de Software</b> e <b>Análise e Desenvolvimento de Sistemas</b><br>
  🔍 Focado em desenvolvimento com <b>C</b> e <b>Python</b> | 🧠 Apaixonado por resolver problemas com código
</p>

---

### 🧠 Sobre mim

Sou um entusiasta de tecnologia e inovação, com foco em **criar soluções práticas e bem estruturadas**.  
Exploro diariamente conceitos de **engenharia de software, lógica de programação, estruturas de dados e bancos de dados**.  
Gosto de transformar ideias em código e código em aprendizado contínuo.

---

### ⚙️ Tecnologias e Ferramentas

<p align="center">
  <img src="https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
  <img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" />
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Linux-333333?style=for-the-badge&logo=linux&logoColor=yellow" />
  <img src="https://img.shields.io/badge/VS_Code-0078D4?style=for-the-badge&logo=visualstudiocode&logoColor=white" />
</p>

---

### 🚀 O que estou desenvolvendo

- 🧩 Aplicações em **C** com foco em algoritmos, estruturas de dados e eficiência  
- 🐍 Projetos em **Python** para automação e integração de sistemas  
- 🧠 Estudos sobre **engenharia de requisitos** e **modelagem de banco de dados**  
- 🧾 Criação de **documentações e portfólios técnicos** organizados

---

### 🎯 Objetivo

Crescer como desenvolvedor e colaborar em projetos que unam **desempenho técnico, aprendizado e propósito real**.  
Busco oportunidades de **estágio** ou **colaboração open-source** para aplicar meus conhecimentos e aprender com profissionais da área.

---

### 🌐 Contato

📍 Brasília - DF, Brasil  
💼 [LinkedIn](https://www.linkedin.com/in/gabriel-mathne-silva-486878377)  
📧 **gabrielmathne@gmail.com**  

---

<p align="center">
  <i>"A melhor maneira de prever o futuro é criá-lo."</i><br>
  — Alan Kay
</p>

<p align="center">
  <img src="https://komarev.com/ghpvc/?username=seu-usuario&color=blue&style=flat-square" alt="Contador de visitas"/>
</p>






#!/usr/bin/env python3
"""
Tic-Tac-Toe (Jogo da Velha) — IA invencível com Minimax + Alpha-Beta
Autor: Gabriel Mathne (modelo por Sebastian)
Execução: python tictactoe.py
"""

from typing import List, Optional, Tuple

HUMAN = "X"
AI = "O"
EMPTY = " "

Board = List[str]


def new_board() -> Board:
    return [EMPTY] * 9


def print_board(b: Board) -> None:
    """Imprime o tabuleiro num layout 3x3 com índices de 1 a 9 para referência."""
    def cell(i):
        return b[i] if b[i] != EMPTY else str(i + 1)
    row = lambda r: f" {cell(3*r+0)} | {cell(3*r+1)} | {cell(3*r+2)} "
    sep = "---+---+---"
    print("\n" + row(0) + f"\n{sep}\n" + row(1) + f"\n{sep}\n" + row(2) + "\n")


def winner(b: Board) -> Optional[str]:
    """Retorna 'X' ou 'O' se houver vencedor; caso contrário None."""
    wins = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8),  # linhas
        (0, 3, 6), (1, 4, 7), (2, 5, 8),  # colunas
        (0, 4, 8), (2, 4, 6)              # diagonais
    ]
    for a, c, d in wins:
        if b[a] != EMPTY and b[a] == b[c] == b[d]:
            return b[a]
    return None


def full(b: Board) -> bool:
    return all(c != EMPTY for c in b)


def legal_moves(b: Board) -> List[int]:
    return [i for i, c in enumerate(b) if c == EMPTY]


def terminal(b: Board) -> bool:
    return winner(b) is not None or full(b)


def score(b: Board, depth: int) -> int:
    """Pontuação do ponto de vista da IA (O). Profundidade recompensa vitórias rápidas."""
    w = winner(b)
    if w == AI:
        return 10 - depth
    if w == HUMAN:
        return depth - 10
    return 0


def minimax(b: Board, player: str, depth: int, alpha: int, beta: int) -> Tuple[int, Optional[int]]:
    """Minimax com poda alpha-beta. Retorna (melhor_score, melhor_movimento)."""
    if terminal(b):
        return score(b, depth), None

    best_move: Optional[int] = None
    if player == AI:
        best_val = -10**9
        for mv in legal_moves(b):
            b[mv] = AI
            val, _ = minimax(b, HUMAN, depth + 1, alpha, beta)
            b[mv] = EMPTY
            if val > best_val:
                best_val, best_move = val, mv
            alpha = max(alpha, val)
            if beta <= alpha:
                break
        return best_val, best_move
    else:
        best_val = 10**9
        for mv in legal_moves(b):
            b[mv] = HUMAN
            val, _ = minimax(b, AI, depth + 1, alpha, beta)
            b[mv] = EMPTY
            if val < best_val:
                best_val, best_move = val, mv
            beta = min(beta, val)
            if beta <= alpha:
                break
        return best_val, best_move


def ai_move(b: Board) -> int:
    """Escolhe o melhor movimento da IA. Garante jogada ótima."""
    _, mv = minimax(b, AI, depth=0, alpha=-10**9, beta=10**9)
    assert mv is not None
    return mv


def human_input(b: Board) -> int:
    """Lê e valida a jogada humana (1..9) e casa livre."""
    while True:
        raw = input("Sua jogada (1-9): ").strip()
        if not raw.isdigit():
            print("⚠️  Digite um número de 1 a 9.")
            continue
        idx = int(raw) - 1
        if idx < 0 or idx > 8:
            print("⚠️  Posição inválida. Use 1 a 9.")
            continue
        if b[idx] != EMPTY:
            print("⚠️  Casa ocupada. Escolha outra.")
            continue
        return idx


def choose_first() -> str:
    while True:
        ch = input("Quem começa? [X você / O IA] (X/O): ").strip().upper()
        if ch in {"X", "O"}:
            return ch
        print("⚠️  Opção inválida. Digite X ou O.")


def game() -> None:
    print("\n=== Tic-Tac-Toe • IA Invencível (Minimax) ===")
    b = new_board()
    turn = choose_first()
    print_board(b)

    while True:
        if turn == HUMAN:
            mv = human_input(b)
            b[mv] = HUMAN
        else:
            print("IA pensando…")
            mv = ai_move(b)
            b[mv] = AI

        print_board(b)

        if winner(b) == HUMAN:
            print("🎉 Você venceu! Jogada perfeita! (ou eu errei? 😅)")
            break
        if winner(b) == AI:
            print("🤖 A IA venceu. Tente outra estratégia!")
            break
        if full(b):
            print("🤝 Deu empate!")
            break

        turn = AI if turn == HUMAN else HUMAN

    again = input("\nJogar novamente? (s/n): ").strip().lower()
    if again == "s":
        game()
    else:
        print("Até a próxima! 👋")


if __name__ == "__main__":
    try:
        game()
    except KeyboardInterrupt:
        print("\nSaindo… 👋")

