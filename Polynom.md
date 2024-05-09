# Polynomial-Manipulation-in-C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 20
typedef struct {
 int coefficient;
 int power;
} Monom;
typedef struct {
 Monom* monoms[SIZE];
 int numOfElements;
} Polynom;
Monom* createMonom(int coefficient, int power) {
 Monom* newMonom = (Monom*)malloc(sizeof(Monom));
 if (coefficient == 0) {
 return NULL;
 }
 else {
 newMonom->coefficient = coefficient;
 newMonom->power = power;
 }
 return newMonom;
}
void addMonom(Polynom* polynom, int coefficient, int power) {
 if (polynom == NULL || polynom->numOfElements >= SIZE || coefficient == 0) {
 return; // Return if polynom is NULL, the array is full, or coefficient is 0
 }
 for (int i = 0; i < polynom->numOfElements; i++) {
 if (polynom->monoms[i]->power == power) {
 // Update the coefficient of an existing monomial with the same power
 polynom->monoms[i]->coefficient += coefficient;
 if (polynom->monoms[i]->coefficient == 0) {
 // If the coefficient becomes zero, free the memory and shift the array
 free(polynom->monoms[i]);
 for (int j = i; j < polynom->numOfElements - 1; j++) {
 polynom->monoms[j] = polynom->monoms[j + 1];
 }
 polynom->numOfElements--;
 }
 return; // Exit the function
  }
 }
 // If no monomial with the same power is found, create a new monomial
 Monom* newMonom = createMonom(coefficient, power);
 if (newMonom != NULL) {
 polynom->monoms[polynom->numOfElements++] = newMonom;
 }
}
void printMonom(Monom* monom) {
 if (monom->power == 0) {
 printf("%d", monom->coefficient);
 }
 else if(monom->power == 1) {
 if (monom->coefficient == 1 || monom->coefficient == -1) {
 printf("x");
 }
 else
 printf("%dx", monom->coefficient);
 }
 else if (monom->coefficient == 1 || monom->coefficient == -1) {
 printf("x^%d", monom->power);
 }
 else {
 printf("%dx^%d", monom->coefficient, monom->power);
 }
}
void printPolynom(Polynom* polynom) {
 for (int i = 0; i < polynom->numOfElements - 1; i++) {
 for (int j = 0; j < polynom->numOfElements - 1 - i; j++) {
 if (polynom->monoms[j]->power < polynom->monoms[j + 1]->power) {
 Monom* temp = polynom->monoms[j];
 polynom->monoms[j] = polynom->monoms[j + 1];
 polynom->monoms[j + 1] = temp;
   }
  }
 }
 int isFirstMonom = 1;
 for (int i = 0; i < polynom->numOfElements; i++) {
 if (polynom->monoms[i]->power > 0) {
 if (!isFirstMonom) {
 printf("+");
  }
 }
 else {
 printf("-");
 }
 printMonom(polynom->monoms[i]);
 isFirstMonom = 0;
 }
 printf("\n");
}
void deletePolynom(Polynom* polynom) {
 for (int i = 0; i < polynom->numOfElements; i++) {
 free(polynom->monoms[i]);
 }
 polynom->numOfElements = 0;
}
int main() {
 Polynom p;
 p.numOfElements = 0;
 addMonom(&p, 4, 3);
 addMonom(&p, 3 ,1);
 addMonom(&p, 1, 1);
 addMonom(&p, 6, 3);
 printPolynom(&p);
 deletePolynom(&p);
 return 0;
}
