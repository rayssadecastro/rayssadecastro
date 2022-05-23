#include <unistd.h> // lib para o delay => usleep()
#include <stdlib.h>
#include <time.h>
#include <ncurses.h>
#define H 25 // altura do mapa
#define W 42 // largura do mapa
#define UP 0
#define RIGHT 1
#define DOWN 2
#define LEFT 3

// Mapa
char map[H][W] = {
{'#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#'},
{'#', 'x', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '#'},
{'#', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', '.', 'O', '#'},
{'#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#', '#'}
};

int game_score = 0;

// Struct do Pacman
typedef struct _TAG_hero {
  int pos[2];
  int direction;
  char face;
} hero;

// Struct dos fantasmas
typedef struct _TAG_ghost {
  int pos[2];
} ghost;

/*
hero pacman
ghost blinky; // RED
ghost pinky; // PINK
ghost inky; // CYAN
ghost clyde; // ORANGE
*/

// Mostrar o mapa.
//  Recebe a posicao do pacman, do blink e, futuramente, dos outros fantasmas
void show_map(int pacman_pos[2], int blinky_pos[2]) {
  int i, j;

  move(0,0); // Mover cursor para a posicao 0,0 da tela

  for (i = 0; i < H; i++) {
    for (j = 0; j < W; j++) {

      if ( j == pacman_pos[0] && i == pacman_pos[1] ) { // se for a posicao do pacman colore
        addch(map[i][j] | COLOR_PAIR(1));
      } else if(j == blinky_pos[0] && i == blinky_pos[1]) { // se for a posicao do blinky
        addch(map[i][j] | COLOR_PAIR(2));
      } else { // se nao for o pacman e nenhum fantasminha
        addch(map[i][j]);
      }
    }
    addch('\n'); // quebra a linha
  }
  mvprintw(H + 1, 0, "SCORE: %d", game_score); // por fim, mostra a pontuacao
  refresh(); // e essa eh a funcao para exibir tudo na tela.
}

// Funcao para mover o pacman no mapa
//  recebe a direcao desejada, a posicao x e y, e qual o caractere dele
void movae_pacman(int direction, int *x, int *y, char face) {

  switch ( direction ) {
    case KEY_UP:
      if ( map[*y - 1][*x] != '#' ) { // verificar se pra cima nao tem parede
        map[*y][*x] = ' '; // colocar espaco em branco por onde ele passou
        if ( map[*y - 1][*x] == '.' ) { // se a posicao desejada tiver um . entao aumenta a pontuacao
          game_score += 10;
        }
        map[--*y][*x] = face; // coloca o pacman na nova posicao
      }
      break;
    // os proximos passos fazem o mesmo que o anterior, mudando apenas a direcao
    case KEY_DOWN:
      if (map[*y + 1][*x] != '#') {
        map[*y][*x] = ' ';
        if ( map[*y + 1][*x] == '.' ) {
          game_score += 10;
        }
        map[++*y][*x] = face;
      }
      break;

    case KEY_LEFT:
      if (map[*y][*x - 1] != '#') {
        map[*y][*x] = ' ';
        if ( map[*y][*x - 1] == '.' ) {
          game_score += 10;
        }
        map[*y][--*x] = face;
      }
      break;

    case KEY_RIGHT:
      if (map[*y][*x + 1] != '#') {
        map[*y][*x] = ' ';
        if ( map[*y][*x + 1] == '.' ) {
          game_score += 10;
        }
        map[*y][++*x] = face;
      }
      break;
  }
}


// Movimentacao do Blinky, codigo que nao esta funcionando
char blinky_last_char = '.';
int blinky_last_pos = -1;

void movae_blinky(int *x, int *y, int pacman_pos[2]) {
  srand( clock() );
  int dir = rand() % 4;

  if ( (*x - pacman_pos[0]) < (*y - pacman_pos[1]) ) {
    if (blinky_last_pos != LEFT) {
      if (map[*y][*x - 1] != '#') {
        map[*y][*x] = blinky_last_char;
        blinky_last_char = map[*y][*x - 1];
        map[*y][--*x] = 'O';
        blinky_last_pos = LEFT;
      }
    }

    if (blinky_last_pos != RIGHT) {
      if (map[*y][*x + 1] != '#') {
        map[*y][*x] = blinky_last_char;
        blinky_last_char = map[*y][*x + 1];
        map[*y][++*x] = 'O';
        blinky_last_pos = RIGHT;
      }
    }
  } else {

  }

}

int main() {

  int key;
  hero pacman;
  ghost blinky/*, pinky, inky, clyde*/;

  pacman.pos[0] = 1; // x
  pacman.pos[1] = 1; // y
  pacman.face = 'x';

  blinky.pos[0] = 40; // x
  blinky.pos[1] = 23; // y

  initscr(); // iniciar a ncurses
  keypad(stdscr, TRUE); // ativar o teclado numerico e setinhas
  curs_set(0); // esconder o cursor da tela
  nodelay(stdscr, TRUE); // ativar o nodelay para nao parar no getch()
  start_color(); // iniciar o sistema de cores da ncurses
  init_pair(1, COLOR_YELLOW, COLOR_BLACK); // cores do pacman
  init_pair(2, COLOR_RED, COLOR_BLACK); // cores do blinky

  show_map(pacman.pos, blinky.pos); // mostrar o mapa

  // Game Loop
  do {
    key = getch(); // pegar tecla do teclado, vai retornar ERR se nao pegar nada (ver nodelay())
    if (key != ERR) { // verificar se pegou alguma tecla, se der ERR foi pq nao pegou
      pacman.direction = key; // atualizar a direcao do pacman
    }

    movae_pacman(pacman.direction, &pacman.pos[0], &pacman.pos[1], pacman.face); // chama a funcao de mover o pacman
    movae_blinky(&blinky.pos[0], &blinky.pos[1], pacman.pos); // chama a funcao de mover o blinky

    show_map(pacman.pos, blinky.pos); // mostra o mapa

    // Delay do jogo
    //  esse if é por causa do tamanho das colunas serem diferentes
    //  do tamanho das linhas, entao para que ele ande na "mesma"
    //  velocidade tem um delay para quando ele estiver movendo na
    //  vertical e outro para quando ele mover na horizontal
    if (pacman.direction == KEY_UP || pacman.direction == KEY_DOWN) {
      usleep(130000); // Delay maior pelo tamanho das colunas
    } else {
      usleep(85000);
    }
  } while (key != 'e');

  curs_set(1); // voltar o cursor para a configuracao inicial
  endwin(); // finalizar a ncurses
  return 0;
}
