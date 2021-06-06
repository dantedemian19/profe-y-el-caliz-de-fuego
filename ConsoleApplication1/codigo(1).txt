#include <iostream>
#include <windows.h>
#include <stdlib.h>
#include <fstream>

using namespace std;
int edad = 0, inte = 0, str = 0;//estadisticas del ultimo alumno ingresado
int i = 0, c = 0;//acumulador y verificador de valores
int strchamp = -1, edcamp = -1, intcamp = -1, numc = -1;// estadisticas del campeon actual
char magia = 'M'; //estadistica de magia del ingresado
char magcamp = 'M';//estadistica de magia del campeon actual

void bienvenido() {//le da una bienvenida al usuario
	cout << "bienvenido al programa Caliz de Fuego \n";
		system("pause");
		system("cls");
};
void sobrescritura() {//sobreescribe los datos del campeon con los del ultimo alumno ingresado 
	numc = i;
	intcamp = inte;
	edcamp = edad;
	strchamp = str;
	magcamp = magia;
};
void pedido(){// le pide al usuario un alumno mayor o igual a 17 años
	c = 0;// verifica que el usuario haya puesto valores validos
	while(c<1){
		c = 0;
		cout <<"alumno "<<i+1<< "____________ \n";
		cout <<"ingrese edad: ";
		cin >> edad;
		system("cls");
		if (edad>=17) {
			do {
				cout << "alumno " << i + 1 << "____________ \n";
				cout << "numero entero entre 0 y 10 (inclusives) \n";
				cout << "ingrese nota de inteligencia del alumno: ";
				cin >> inte;
				system("cls");
				if (inte >= 0 && inte <= 10) c = 1;
				else cout << "el valor es invalido ingrese otra vez \n";
			} while (c != 1);
			do{
				c = 0;
				cout << "alumno " << i + 1 << "____________ \n";
				cout << "numero entero entre 0 y 10 (inclusives) \n";
				cout << "ingrese nota de fuerza del alumno: ";
				cin >> str;
				system("cls");
				if (str >= 0 && str <= 10) c = 1;
				else cout << "el valor ingresado es invalido, ingrese un valor valido \n";
			} while (c != 1);
			do {
				c = 0;
				cout << "alumno " << i + 1 << "____________ \n";
				cout << "(B)uena / (R)egular / (M)ala \n";
				cout << "ingrese nota en magia del alumno: ";
				cin >> magia;
				system("cls");
				if(magia=='B'|| magia == 'R' || magia == 'M' ) c = 1;
				else cout << "el valor ingresado es invalido, ingrese un valor valido \n";
			} while (c != 1);
			c = 1;
		}
		else {// le da un aviso de que el alumno es muy joven para participar
			cout << "su alumno es muy joven para participar \n";
			i = i - 1;
			c = 1;
		}
	}
};
void comparador() {//compara si el alumno ingresado es mejor que el campeon actual
	if (inte > intcamp) {//comparacion por nota de inteligencia entre ambos
		sobrescritura();
	}
	else {
		if (str>strchamp&&inte==intcamp) {//comparacion por nota de fuerza, en caso de que la inteligencia sea igual entre ambos
			sobrescritura();
		}
		else {// cambia el valor de la magia de una letra a un numero
			int champm=0, mag=1;
			if (magcamp == 'M') {
				champm = 1;
			}if (magcamp == 'R') {
				champm = 2;
			}if (magcamp == 'B') {
				champm = 3;
			}
			if (magia == 'M') {
				mag = 1;
			}if (magia == 'R') {
				mag = 2;
			}if (magia == 'B') {
				mag = 3;
			}


			if (mag>champm&&str == strchamp && inte == intcamp) {//comparacion por nota de magia, en caso de que la inteligencia y la fuerza sean iguales entre ambos
				sobrescritura();
			}
		}
	}
};
void mostrar() { //muestra el resultado del programa 
	if (numc != -1) {//muestra el numero de campeon, si hay
		cout << "El campeon es el numero " << numc + 1 << " :)\n";
	}
	else{//muestra que no hay campeon
		cout << "No hay campeon :( \n";
	}
};
void control() {//verifica si el usuario desea ingresar mas alumnos
	bool b = true;// mantiene al usuario en la funcion pedido hasta que no desee ingresar alumnos
	bool w = true;// verifica que el usuario haya puesto un caracter valido
	char comp='a';//valor que ingresa el usuario
	do {
		w = true;
		cout << "desea ingresar un alumno? \n" << "pulse Y para si, pulse N para no \n";
		cin >> comp;
		if (comp == 'N' || comp == 'n')b=false;
		if (comp == 'y' || comp == 'Y' || comp == 'N' || comp == 'n')w = false;
		system("cls");
	} while (w);
	while (b) {
		pedido();
		do {//verifica si el usuario desea ingresar otro alumno
			w = true;
			cout << "desea ingresar otro alumno? \n" << "pulse Y para si, pulse N para no \n";
			cin >> comp;
			if (comp == 'N' || comp == 'n')b = false;
			if (comp == 'y' || comp == 'Y'|| comp == 'N' || comp == 'n')w = false;
			system("cls");
		}while (w);
		if (edad>=17) {//verifica si el usuario ingreso a un alumno valido para campeon
			comparador();
		}
		i = i + 1;
	}
};
//la funcion archivo remplaza el pedido de datos al usuario, por un archivo con datos de alumnos
// si desea probar el metodo por archivo de texto, comente la funcion control(); en el main y descomente archivo();
// el archivo de texto se llama test.txt y se ingresa (edad inte str magia) por cada linea es un alumno 
void archivo() {
	fstream data;
	data.open("test.txt");
	if (!data.fail()) {
		while (!data.eof()) {
			data >> edad >> inte >> str >> magia;
			if (edad >= 17) {
				comparador();
				i = i + 1;
			}
		}
		
	}
	else {
		cout << "el archivo fallo.....";
		exit(EXIT_FAILURE);
	}
	data.close();
};

int main(){ 
	bienvenido();
	//archivo();
	control();
	mostrar();
}