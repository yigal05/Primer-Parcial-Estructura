# Primer-Parcial-Estructura

## Nombre de quien presenta: Yigal Rojas Acevedo

## Codigo: 1061368791

### Pregunta 1

## 1

Crear nuevos arreglos y almacenarlos en las nuestro vector es una solucion posible pero poco efieciente , para empezar tenemos que saber que un vector tiene sus caracteristicas dos de sus mayores caracteristicas son que son dinamicaas ( su tamaño o capacidad de almacenamiento aumenta o dismunuye segun se requiera) , y su otra virtud es que sus datos almacenados de forma consecutiva , es decir cada dato que se crea deber estar almacenado despues del otro.
Entonces respondiendo la pregunta ¿ Por que es necesario hacer un resize? , el resize es necesario para decirle al sistema que queremos seguir utilizando un vector pero ahora de un mayor tamaño que ha sobrepasado el actual , de esta manera seguimos teniendo los datos de manera consecutiva.

## 2

Ambos tiene sus ventajas , Consierando que una lista no requiere hacer un resize , lo que conlleva a crear nuevas lista y copiar los datos anteriores , si no que puede ir creando y eliminando cada uno de sus nodos , teniendo esto en cuenta y sabiendo que las demas operaciones tiene una misma complejidad , desde mi punto de vista , las lista si son mas eficientes que los vectores . Tambien una gran consieracion es que con una lista nunca tendremos espacio innecesario o siendo desperdciado , como es el caso de los vectores.

### Pregunta 2

## 1

// la capacidad final de x creado con un almacenamiento inical de 5 y un una escalabilidad de 2 es de 10240 su desperdicio seria entonces 10240-10000 es decir 240 utilizando 11 resizes;
// la capacidad final de y creado con un almacenamiento inical de 10 y un una escalabilidad de 1.8 es de 11178 su desperdicio seria entonces 11178-10000 es decir 1178; utilizando 12 resize;
// la capacidad final de y creado con un almacenamiento inical de 10 y un una escalabilidad de 2 es de 12800 su desperdicio seria entonces 12800-10000 es decir 2800 utlizadon 8 resize

## 2

Teniendo en cuenta la cantidad de resize que hizo el vector mas eficiente fue el que aumentaba de 2 en dos con una capacidad de inicial de 100 es decir , el Z, aunque bien este ha sacrificado la cantdad de desperdicio , siendo también el que mas deperdicia , esto es claramente comprensible ya que su capcadad de aumento por inicar en 100 e ir duplicandose lleguera de manera mas repaida a 10000 pero asi mismo ir aumentando de manera doble.
basicamente su eficiencia se basa en tener constantemente mas espacio disponible.

### Pregunta 3

#include <iostream>
#include <cassert>

using namespace std;


template <typename T>
class Vector{
    private:
        unsigned int size;
        unsigned int capacity;
        T *storage; 

        void resize(){
            unsigned int newCapacity = 1 + (capacity*1.5);
            T* newStorage = new T[newCapacity];

            for(unsigned int i=0; i < size; i++){
                newStorage[i] = storage[i];
            }

            delete[] storage;
            storage = newStorage;
            capacity = newCapacity;
        }

        void resize(unsigned int newCapacity){

            T* newStorage = new T[newCapacity];

            for(unsigned int i=0; i < size; i++){
                newStorage[i] = storage[i];
            }

            delete[] storage;
            storage = newStorage;
            capacity = newCapacity;
        }

    public:

        unsigned int getSize(){ return size; }
        unsigned int getCapacity(){ return capacity; }
        
        Vector() : size(0), capacity(1), storage(new T[capacity]) {}
        
        ~Vector(){
            delete []storage;
        }

        Vector(Vector<T>* vec){
            size = vec->getSize();
            capacity = vec->getCapacity();
            storage = new T[capacity];     
                   
            for(unsigned int i=0; i<size; i++){
                storage[i] = vec->at(i);
            }
        }

        void push_back(T elem){
            if( size == capacity){ resize(); }
            storage[size]= elem;
            size++;
        }

        void push_front(T elem){
            push_back(elem);

            for(unsigned int i=size; i>0; i--){
                storage[i] = storage[i-1];
            }

            storage[0] = elem; 
        }

        T at(unsigned int elem){
            if(elem < size){
                return storage[elem];
            }else{
                return -1;
            }
        }

        void pop_back(){
            delete storage[size-1];
            size--;
        }

        void insert(T elem, unsigned int pos){
            push_back(elem);

            for(unsigned int i=size-1; i>pos; i--){
                storage[i] = storage[i-1];
            }

            storage[pos] = elem; 
        }
        
        // [ from, to ) 
        void remove(unsigned int from, unsigned int to){
            unsigned int range = to - from;

            for(unsigned int i=from; i<size-range; i++){
                storage[i] = storage[i+range];
            }

            size = size - range; 
        }

        void Print(){
            if(size == 0){
                cout << "{ }" ;
            }
            else{
                cout << "{ ";
                for(unsigned int i=0; i < size-1; i++){
                    cout << storage[i] << ", ";
                } 
                cout << storage[size-1] << " }";
            }
        }

        void pop_front(){
            for (unsigned int i; i < size-1; i++){
                storage[i]= storage[i+1];
            }
            pop_back();
        }
        
        void remove(unsigned int target){
            for(unsigned int i =target; i<size-1;i++){
                storage[i]= storage[i+1];
            }
            pop_back();
        }

        void insert( Vector<T>* other , unsigned int pos){
            for ( unsigned int i = 0 ;  i < other->getSize(); i++){
                insert(other->at(i),pos+i);
            }
        }
};


template <typename T>
class List{
    private:
        class Node{
            private:
                T data;
                Vector<Node*> conexiones;
            public:
                Node(T elem) : data(elem), conexiones(nullptr) {}
                T getData(){ return data;}
            
        };
        
        Node *first; 
        Node *last;
        unsigned int size; 

    public:
        List() : first(nullptr), last(nullptr), size(0) {}
        
        ~List() {
            Node* n = first; 

            while(n != nullptr){
                first  = n->getNext();
                delete n;
                n = first; 
            }

            first = nullptr;
            last = nullptr; 
        }

        unsigned int getSize(){ return size; }
        
        bool empty(){ return first == nullptr; }


        void remove(unsigned int pos){
            //remueve cierta poscion de esta lista
        }



        void Print(){
            // imprime todos los elementos de esta lista
        }

        void addCity(T elem){
            //añade una un elemento a la lista
        }

        void addcity( unsigned int elem , unsigned int other  ){
            // crea una conexion entre el elemento de esta posicion y el elemento de otra 
            //esto lo hace añadiendo el puntero de other el al vector de punteros que tiene elem;
        }

        void PrintConexiones( unsignet int elem){
            // imprime los elementos con los que hace conexion el elemento es decir el vecor conexiones
        }
        
        (Estas son anotaciones mias pero no alcance implementar)

};

