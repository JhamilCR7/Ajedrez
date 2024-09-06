#include <iostream>
#include <vector>
using namespace std;

class pieza {
protected:
    short fila;
    short columna;

public:
    pieza() : fila(1), columna(1) {}  

    pieza(short f, short c) : fila(f), columna(c) {}

 
    short getFila() {
        return fila;
    }

    short getColumna() {
        return columna;
    }

    void setFila(short f) {
        fila = f;
    }

    void setColumna(short c) {
        columna = c;
    }

    virtual void verInfo() = 0;

    
    bool esValido(short f, short c) {
        return (f >= 1 && f <= 8 && c >= 1 && c <= 8);
    }

  
    virtual bool puedoIrA(short f, short c) = 0;

 
    virtual char representar() = 0;
};


class Peon : public pieza {
private:
    short direccion;

public:
    Peon(short f, short c, short dir) : pieza(f, c), direccion(dir) {}

    void avanzar() {
        if (direccion == 1) {
            fila++;
        }
        else if (direccion == -1) {
            fila--;
        }
    }

    bool puedoIrA(short f, short c) override {
        return (f == fila + direccion && c == columna);
    }

    void verInfo() override {
        cout << "Peon en Fila: " << fila << ", Columna: " << columna << endl;
    }

    char representar() override {
        return 'X';
    }
};


class Torre : public pieza {
public:
    Torre(short f, short c) : pieza(f, c) {}

    bool puedoIrA(short f, short c) override {
        return (fila == f || columna == c);
    }

    void verInfo() override {
        cout << "Torre en Fila: " << fila << ", Columna: " << columna << endl;
    }

    char representar() override {
        return 'T';
    }
};


class Tablero {
private:
    vector<vector<pieza*>> tablero;  

public:
    Tablero() {
        
        tablero.resize(8, vector<pieza*>(8, nullptr));
    }

    void colocarPieza(pieza* p, short f, short c) {
        if (p->esValido(f, c)) {
            tablero[f - 1][c - 1] = p;
            p->setFila(f);
            p->setColumna(c);
            cout << "Pieza colocada en " << f << ", " << c << endl;
        }
        else {
            cout << "Posicion invalida" << endl;
        }
    }


    void verTablero() {
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                if (tablero[i][j] == nullptr) {
                    cout << "[ ] ";
                }
                else {
                    cout << "[" << tablero[i][j]->representar() << "] ";
                }
            }
            cout << endl;
        }
    }

    
    void moverPieza(short f1, short c1, short f2, short c2) {
        pieza* p = tablero[f1 - 1][c1 - 1];
        if (p != nullptr && p->puedoIrA(f2, c2) && p->esValido(f2, c2)) {
            tablero[f2 - 1][c2 - 1] = p;
            tablero[f1 - 1][c1 - 1] = nullptr;
            p->setFila(f2);
            p->setColumna(c2);
            cout << "Pieza movida a " << f2 << ", " << c2 << endl;
        }
        else {
            cout << "Movimiento invalido" << endl;
        }
    }
};

int main() {
    Tablero t;
    Peon* peon = new Peon(2, 2, 1);
    Torre* torre = new Torre(1, 1);

    t.colocarPieza(peon, 2, 2);
    t.colocarPieza(torre, 1, 1);

    t.verTablero();

    short f1, c1, f2, c2;
    cout << "Mover Peon (X): Ingrese fila y columna actuales y luego fila y columna destino: ";
    cin >> f1 >> c1 >> f2 >> c2;
    t.moverPieza(f1, c1, f2, c2);
    t.verTablero();

    cout << "Mover Torre (T): Ingrese fila y columna actuales y luego fila y columna destino: ";
    cin >> f1 >> c1 >> f2 >> c2;
    t.moverPieza(f1, c1, f2, c2);
    t.verTablero();

    delete peon;
    delete torre;

    return 0;
}
