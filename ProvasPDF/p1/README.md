# Respostas
## 1

## 2
```C
void *request (void *args) {
    int random_number = (rand() % 100) + 1; // // Obtain a number between [1 - 100].
    sleep(random_number); // Sleeps seconds
    pthread_exit(random_number);
}

int gateway (int nthreads, int wait_nthreads) {
    pthread_t pthreads[nthreads];
    for (int i = 0; i < nthreads; i++) {
        pthread_create (&pthreads[i], NULL, &request, (void*) i);
    }
    
    pthread_cond_wait(pthread_cont_t *cv,pthread_mutex_t *mutex); // duvida

    int sum = 0;
    for (int i = 0; i < nthreads; i++) {
        void* aux;
        pthread_join(pthreads[i], &aux);    
        sum += (int) aux;
    }
    return sum;
}
```

## 3

```C
// RESOLVER ENVOLVENDO OS MUTEX NOS LUGARES CORRETOS
// basic node structure  
typedef struct __node_t {  
    int key;  
    struct __node_t *next;
 } node_t;

// basic list structure (one used per list)
typedef struct __list_t {
       node_t *head;
} list_t;

void List_Init(list_t *L) {
      L->head = NULL
}

int List_Insert(list_t *L, int key) {
      node_t* new = malloc(sizeof(node_t));
      if (new == NULL) {
          perror("malloc");
          return -1; // fail
      }
      new->key = key;
      new->next = L->head;
      L->head = new;
      return 0; // success
}

int List_Lookup(list_t*L, int key) {
    pthread_mutex_lock(&L->lock);
    node_t*curr = L->head;
    while (curr) {
        if (curr->key == key){
            return 0; // success
        }
        curr =curr->next;
    } 
    return -1; // failure
} 
```