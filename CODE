#include <allegro5/allegro.h>
#include <allegro5/allegro_image.h>
#include <allegro5/allegro_font.h>
#include <allegro5/allegro_ttf.h>
#include <allegro5/allegro_primitives.h>
#include <allegro5/allegro_native_dialog.h>
#include <allegro5/allegro_image.h>
#include <allegro5/allegro_audio.h>
#include <allegro5/allegro_acodec.h>
#include <stdlib.h>
#include <math.h>
#include <stdio.h>
#include <time.h>

#define N_LINHAS 6 //5
#define N_COLUNAS 6 //5
#define NUM_TYPES 6

#define CANDY1 1
#define CANDY2 2
#define CANDY3 3
#define CANDY4 4
#define CANDY5 5
#define CANDY6 6

#define LARGURA_TELA 720
#define ALTURA_TELA 720
#define LIN_W 120;
#define COL_W 120;
#define INFO_H 120//tamanho da celula
#define FPS 1
//#define MARGIN 5


//#define LARGURA_JOGO LARGURA_TELA-64
//#define ALTURA_JOGO ALTURA_TELA-64


//estruturas
typedef struct Candy {
	int type; //tipo de doce
	int offset_lin;
	int offset_col;
	int active;
} Candy;

Candy M[N_LINHAS][N_COLUNAS];

const float CELL_W = (float)(LARGURA_TELA)/N_COLUNAS;
const float CELL_H = (float)(ALTURA_TELA)/N_LINHAS; //espaço

int score=0, plays=20;
char my_score[100], my_plays[100];

int newRecord(int score, int *record) {
	FILE *arq = fopen("recorde.txt", "r"); //n coloquei arquivo nenhum. erro? Não, o arquivo cria sozinho. read file
	*record = -1;
	if(arq != NULL) {
		fscanf(arq, "%d", record);
		fclose(arq);
	}
	if(*record < score ) {
		arq = fopen("recorde.txt", "w"); //write file
		fprintf(arq, "%d", score);
		fclose(arq);
		return 1;
	}
	return 0;

}

ALLEGRO_FONT *size_f;

int generateRandomCandy() {
	return rand()%NUM_TYPES + 1;
}

int getXCoord(int col){
	return col * COL_W;
}

int getYCoord(int linha){
	return linha * LIN_W;
}

/*void imprimeMatriz() {
	printf("\n");
	int i, j;
	for(i=0; i<N_LINHAS; i++) {
		for(j=0; j<N_COLUNAS; j++) {
			printf("%d (%d,%d) ", M[i][j].type, M[i][j].offset_lin, M[i][j].active);
		}
		printf("\n");
	}
}*/


void completaMatriz() {
	int i, j;
	for(i=0; i<N_LINHAS; i++) {
		for(j=0; j<N_COLUNAS; j++) { 
			if(M[i][j].type == 0) {
				M[i][j].type = generateRandomCandy();
				M[i][j].offset_col = 0;
				M[i][j].offset_lin = 0;
				M[i][j].active = 1;

			}
		}
	
	}
}

void initGame() {
	int i, j;
	for(i=0; i<N_LINHAS; i++) {
		for(j=0; j<N_COLUNAS; j++) {
			M[i][j].type = generateRandomCandy();
			M[i][j].offset_col = 0;
			M[i][j].offset_lin = 0;
			M[i][j].active = 1;
			//M[i][j].cor = colors[M[i][j].type];
			printf("%d ", M[i][j].type);
		}
		printf("\n");
	}
	for(i=0; i<N_LINHAS; i++) {
		for(j=0; j<N_COLUNAS; j++) { 
			printf("%d ", M[i][j].type);
		}
		printf("\n");
	}
	printf("\n");
}

void pausa(ALLEGRO_TIMER *timer) {
	al_stop_timer(timer);
	al_rest(3);
	al_start_timer(timer);
}

void draw_candy(int lin, int col) {

	int x0 = getXCoord(col);
	int y0 = getYCoord(lin);

	/*int cell_x = CELL_W*col;
	int cell_y = INFO_H + CELL_H*lin;*/

	ALLEGRO_BITMAP *candy1 = NULL;
	ALLEGRO_BITMAP *candy2 = NULL;
	ALLEGRO_BITMAP *candy3 = NULL;
	ALLEGRO_BITMAP *candy4 = NULL;
	ALLEGRO_BITMAP *candy5 = NULL;
	ALLEGRO_BITMAP *candy6 = NULL;
	

	candy1 = al_load_bitmap("candy1.png");
	candy2 = al_load_bitmap("candy2.png");
	candy3 = al_load_bitmap("candy3.png");
	candy4 = al_load_bitmap("candy4.png");
	candy5 = al_load_bitmap("candy5.png");
	candy6 = al_load_bitmap("candy6.png");

	if(M[lin][col].type == CANDY1) {
		al_draw_bitmap(candy1, x0, y0, 0);
	}
	else if(M[lin][col].type == CANDY2) {
		al_draw_bitmap(candy2, x0, y0, 0);

	}
	else if(M[lin][col].type == CANDY3) {
		al_draw_bitmap(candy3, x0, y0, 0);
	}
	else if(M[lin][col].type == CANDY4) {
		al_draw_bitmap(candy4, x0, y0, 0);
	}

	else if(M[lin][col].type == CANDY5) {
		al_draw_bitmap(candy5, x0, y0, 0);
	}
	else if(M[lin][col].type == CANDY6) {
		al_draw_bitmap(candy6, x0, y0, 0);
	}

	al_destroy_bitmap(candy1);
	al_destroy_bitmap(candy2);
	al_destroy_bitmap(candy3);
	al_destroy_bitmap(candy4);
	al_destroy_bitmap(candy5);
	al_destroy_bitmap(candy6);

}

void draw_scenario(ALLEGRO_DISPLAY *janela) {


	ALLEGRO_COLOR BKG_COLOR = al_map_rgb(0,0,0);
	al_set_target_bitmap(al_get_backbuffer(janela));
	al_clear_to_color(BKG_COLOR);

	//SCORE
	sprintf(my_score, "%d", score);
	al_draw_text(size_f, al_map_rgb(0, 0, 0), 180, 275, 0, my_score);
	//PLAYS
	sprintf(my_plays, "%d", plays);
	al_draw_text(size_f, al_map_rgb(0, 0, 0), 180, 405, 0, my_plays);

	int i, j;
	for(i=0; i<N_LINHAS; i++) {
		for(j=0; j<N_COLUNAS; j++) {
			draw_candy(i, j);
		}
	}
	printf("%d ", M[i][j].type);

}

int clearSequence(int li, int lf, int ci, int cf) {
	int i, j, count=0;
	for(i=li; i<=lf; i++) {
		for(j=ci; j<=cf; j++) {
			count++;
			M[i][j].active = 0;
		}
	}
	return count;
}

int processaMatriz() {
	//retorna a quantidade de pontos feitos
	int i, j, k, count = 0;
	int current, seq, ultimo;

	//procura na horizontal:
	for(i=0; i<N_LINHAS; i++) {
		current = M[i][0].type;
		seq = 1;
		for(j=1; j<N_COLUNAS; j++) {
			if(current == M[i][j].type && current > 0) {
				seq++;
				if(j == N_COLUNAS-1 && seq >=3)
					count += clearSequence(i, i, j-seq+1, N_COLUNAS-1);
			}
			else {
				if(seq >= 3) {
					count += clearSequence(i, i, j-seq, j-1);
				    //printf("\n1: (%d, %d), seq: %d, count: %d", i, j, seq, count);
				}
				seq = 1;
				current = M[i][j].type;
			}
		}
		
	}


	//procura na vertical:
	for(j=0; j<N_COLUNAS; j++) {
		current = M[0][j].type;
		seq = 1;
		for(i=1; i<N_LINHAS; i++) {
			if(current == M[i][j].type && current > 0) {
				seq++;
				if(i == N_LINHAS-1 && seq >=3)
					count += clearSequence(i-seq+1, N_LINHAS-1, j, j);
			}
			else {
				if(seq >= 3) {
					count += clearSequence(i-seq, i-1, j, j);
					//printf("\n2: (%d, %d), seq: %d, count: %d", i, j, seq, count);
				}

				seq = 1;
				current = M[i][j].type;

			}
		}
		

	}


	return count;


}

void atualizaOffset() {
	int i, j, offset;

	for(j=0; j<N_COLUNAS; j++) {
		offset = 0;
		for(i=N_LINHAS-1; i>=0; i--) {
			M[i][j].offset_lin = offset;
			if(M[i][j].active == 0) {
				M[i][j].type = 0;
				offset++;
				//M[i][j].active = 1;
			}
			//else {
			//	M[i][j].offset_lin = offset;
			//}
		}
	}
	
}

void atualizaMatriz() {
	int i, j, offset;

	for(j=0; j<N_COLUNAS; j++) {
		for(i=N_LINHAS-1; i>=0; i--) {
			offset = M[i][j].offset_lin;
			if(offset > 0) {
				M[i+offset][j].type = M[i][j].type;
				M[i+offset][j].active = M[i][j].active;
				M[i][j].type = 0;
				M[i][j].active = 0;
				M[i][j].offset_lin = 0;
			}
		}
	} 
	for(i=0; i<N_LINHAS; i++) {
		for(j=0; j<N_COLUNAS; j++) { 
			printf("%d ", M[i][j].type);
		}
		printf("\n");
	}
	printf("\n");

}

void getCell(int x, int y, int *lin, int *col) {
	/*x-=400;
	y-=60;
	*lin = y/LIN_W;
	*col = x/COL_W;*/
	//*lin = (y-INFO_H)/CELL_H;
	*lin = y/CELL_H;
	*col = x/CELL_W;
}

int distancia(int lin1, int col1, int lin2, int col2) {
	return sqrt(pow(lin1-lin2, 2) + pow(col1-col2, 2));
}

void troca(int lin1, int col1, int lin2, int col2) {
	int l, c;
	Candy aux = M[lin1][col1];
	M[lin1][col1] = M[lin2][col2];
	M[lin2][col2] = aux;
	plays--;

	for(l=0; l<N_LINHAS; l++) {
		for(c=0; c<N_COLUNAS; c++) { 
			printf("%d ", M[l][c].type);
		}
		printf("\n");
	}
	printf("\n");
}


int main(int argc, char **argv) {

	ALLEGRO_DISPLAY *janela = NULL;
	ALLEGRO_EVENT_QUEUE *event_queue = NULL;
	ALLEGRO_TIMER *timer = NULL;
	//samples que guardam os efeitos sonoros
	ALLEGRO_SAMPLE *laser =NULL;
	ALLEGRO_SAMPLE *fundo =NULL;
	ALLEGRO_SAMPLE *vitoria =NULL;
	ALLEGRO_SAMPLE *game_over =NULL;

	ALLEGRO_SAMPLE_INSTANCE *instancia1 = NULL;
    ALLEGRO_SAMPLE_INSTANCE *instancia2 = NULL;
    ALLEGRO_SAMPLE_INSTANCE *instancia3 = NULL;
    ALLEGRO_SAMPLE_INSTANCE *instancia4 = NULL;




 //--------------------------------------Rotinas de Inicialização-----------------------

	//função que inicializa a biblioteca allegro e condicial de erro
	al_init();
	if (!al_init()){
		fprintf(stderr, "Falha ao inicializar o allegro!\n");
		return -1;
	}

	if(!al_init_primitives_addon()){
		fprintf(stderr, "failed to initialize primitives!\n");
		return -1;
	}

	timer = al_create_timer(1.0 / FPS);
	if(!timer) {
		fprintf(stderr, "failed to create timer!\n");
		return -1;
	}
	//cria tela e condicional de erro
	janela = al_create_display(LARGURA_TELA, ALTURA_TELA);
	if (!janela){
		fprintf(stderr, "Falha ao criar janela!\n");
		al_destroy_timer(timer);
		return -1;
	}

	if(!al_install_mouse()){
		fprintf(stderr, "failed to initialize mouse!\n");
		return -1;
	}

	// Inicializa o add-on para utilização de imagens
    if (!al_init_image_addon()){
        fprintf(stderr, "Falha ao inicializar o addon imagem!\n");
		return -1;
    }

    if(!al_install_audio()){
        fprintf(stderr, "Falha ao inicializar o audio\n");
        return -1;
    }
 
    //addon que da suporte as extensoes de audio
    if(!al_init_acodec_addon()){
        fprintf(stderr, "Falha ao inicializar o codec de audio \n");
        return -1;
    }

    laser = al_load_sample( "laser.ogg" );
    if (!laser){
        fprintf(stderr, "Audio nao carregado" );
        return -1;
     }


    fundo = al_load_sample( "fundo.ogg" );
    if (!laser){
        fprintf(stderr, "Audio nao carregado" );
        return -1;
     }

    if (!al_reserve_samples(10.0)){
        fprintf(stderr, "failed to reserve samples!\n");
        return -1;
    }

      vitoria = al_load_sample("vitoria.ogg");
    if (!laser){
        fprintf(stderr, "Audio vitoria nao carregado");
        return -1;
     }

    game_over = al_load_sample("game_over.ogg");
    if (!laser){
        fprintf(stderr, "Audio game_over nao carregado");
        return -1;
     }

	//inicializa o modulo allegro que carrega as fontes
	al_init_font_addon();
	//inicializa o modulo allegro que entende arquivos tff de fontes
	al_init_ttf_addon();

    //carrega o arquivo arial.ttf da fonte Arial e define que sera usado o tamanho 32 (segundo parametro)
	size_f = al_load_font("arial.ttf", 32, 1);

	event_queue = al_create_event_queue();
	if(!event_queue) {
		fprintf(stderr, "failed to create event_queue!\n");
		al_destroy_display(janela);
		al_destroy_timer(timer);
		return -1;
	}

	instancia1 = al_create_sample_instance(laser);
	al_attach_sample_instance_to_mixer(instancia1, al_get_default_mixer());

	instancia2 = al_create_sample_instance(fundo);
	al_attach_sample_instance_to_mixer(instancia2, al_get_default_mixer());

	instancia3 = al_create_sample_instance(vitoria);
	al_attach_sample_instance_to_mixer(instancia3, al_get_default_mixer());

	instancia4 = al_create_sample_instance(game_over);
	al_attach_sample_instance_to_mixer(instancia4, al_get_default_mixer());

	al_install_keyboard();
   //registra na fila de eventos que eu quero identificar quando a tela foi alterada
	al_register_event_source(event_queue, al_get_display_event_source(janela));
   //registra na fila de eventos que eu quero identificar quando o tempo alterou de t para t+1
	al_register_event_source(event_queue, al_get_timer_event_source(timer));
	//registra o teclado na fila de eventos:
	al_register_event_source(event_queue, al_get_keyboard_event_source());
	//registra mouse na fila de eventos:
	al_register_event_source(event_queue, al_get_mouse_event_source());
   //inicia o temporizador
	al_start_timer(timer);

	//----------------------- fim das rotinas de inicializacao ---------------------------------------



	srand(2);
	initGame();
	int n_zeros = processaMatriz();
	while(n_zeros > 0) {
		do {
			atualizaOffset();
			//imprimeMatriz();
			atualizaMatriz();
			//imprimeMatriz();
		} while(processaMatriz());
		completaMatriz();
		//imprimeMatriz();
		n_zeros = processaMatriz();
		//imprimeMatriz();
		//system("PAUSE");
	}

	draw_scenario(janela);
	al_play_sample_instance(instancia1);
	al_flip_display();
/*	printf("\n# de zeros: %d", processaMatriz());
	pausa(timer);
	atualizaOffset();
	imprimeMatriz();
	pausa(timer);
	atualizaMatriz();
	imprimeMatriz();
	pausa(timer);
	completaMatriz();
	imprimeMatriz();
	pausa(timer);
	printf("\n# de zeros: %d", processaMatriz());
	imprimeMatriz();
	system("PAUSE");		*/
	int pontos, playing = 1, col_src, lin_src, col_dst, lin_dst, flag_animation=0;
	//enquanto playing for verdadeiro, faca:

	while(playing) {
		ALLEGRO_EVENT ev;
	  //espera por um evento e o armazena na variavel de evento ev
		al_wait_for_event(event_queue, &ev);
		al_play_sample_instance(instancia2);

		if(ev.type == ALLEGRO_EVENT_KEY_DOWN) {
			if(ev.keyboard.keycode == ALLEGRO_KEY_ESCAPE) {
				playing = 0;
			}

		}
		else if(ev.type == ALLEGRO_EVENT_MOUSE_BUTTON_DOWN) {   //tirei o !flaganimation
			lin_src = rand() % 6;
			col_src = rand() % 6;
			lin_dst = rand() % 6;
			col_dst = rand() % 6;
			troca(lin_src, col_src, lin_dst, col_dst);
			getCell(ev.mouse.x, ev.mouse.y, &lin_src, &col_src);
		}
		else if(ev.type == ALLEGRO_EVENT_MOUSE_BUTTON_UP) {//aqui tbm
			getCell(ev.mouse.x, ev.mouse.y, &lin_dst, &col_dst);
			if((distancia(lin_src, col_src, lin_dst, col_dst) == 1)
				&& (M[lin_src][col_src].type != M[lin_dst][col_dst].type)) {
				troca(lin_src, col_src, lin_dst, col_dst);
				al_play_sample_instance(instancia1);
				//imprimeMatriz();
//				flag_animation = 1; //nao permite que o usuario faca outro comando enquanto a animacao ocorre
			}

		}
	    //se o tipo de evento for um evento do temporizador, ou seja, se o tempo passou de t para t+1
		else if(ev.type == ALLEGRO_EVENT_TIMER) {
			pontos = processaMatriz();

			/*while(pontos > 0) {
				do {
					score += pontos;
				    draw_scenario(display);
					al_flip_display();
					pausa(timer);
					atualizaOffset();
					//imprimeMatriz();
					atualizaMatriz();
					//imprimeMatriz();
					pontos = processaMatriz();
				} while(pontos > 0);
				completaMatriz();
				//imprimeMatriz();
				pontos = processaMatriz();
				//imprimeMatriz();
				//system("PAUSE");
			} */


			while(pontos > 0) {
				//score+=pow(2,pontos);
			    draw_scenario(janela);
				al_flip_display();
				pausa(timer);
				//imprimeMatriz();
				atualizaOffset();
				//imprimeMatriz();
				atualizaMatriz();
				//imprimeMatriz();
				completaMatriz();
				score+=pow(2,pontos);
				pontos = processaMatriz();
				//printf("\n%d ", pontos);
			}

		    //reinicializo a tela
		    draw_scenario(janela);
			al_flip_display();
			//if(pontos > 0) {
			//	score += pontos;
				//pausa(timer);

			//}
			if(plays == 0)
				playing = 0;
			flag_animation = 0;
			//printf("\nflag_animation: %d", flag_animation);

		}
	    //se o tipo de evento for o fechamento da tela (clique no x da janela)
		else if(ev.type == ALLEGRO_EVENT_DISPLAY_CLOSE) {
			playing = 0;
		}

	}

	al_rest(1);

	int record;
	//colore toda a tela de preto
	al_clear_to_color(al_map_rgb(230,240,250));
	sprintf(my_score, "Score: %d", score);
	al_draw_text(size_f, al_map_rgb(200, 0, 30), LARGURA_TELA/3, ALTURA_TELA/2, 0, my_score);
	if(newRecord(score, &record)) {
		al_destroy_sample(fundo);
		al_draw_text(size_f, al_map_rgb(200, 20, 30), LARGURA_TELA/3, 100+ALTURA_TELA/2, 0, "NEW RECORD!");
		al_play_sample_instance(instancia3);
	}
	else {
		al_destroy_sample(fundo);
		sprintf(my_score, "Record: %d", record);
		al_draw_text(size_f, al_map_rgb(0, 200, 30), LARGURA_TELA/3, 100+ALTURA_TELA/2, 0, my_score);
		al_play_sample_instance(instancia4);
	}
	//reinicializa a tela
	al_flip_display();
	al_rest(6);


	al_destroy_timer(timer);
	al_destroy_display(janela);
	al_destroy_sample(laser);
    al_destroy_sample(vitoria);
    al_destroy_sample(game_over);
	al_destroy_event_queue(event_queue);

	return 0;
}
