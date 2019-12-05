# Respostas
## 1

```Java
public class BlockingQueue {

    private List queue = new LinkedList();
    private int limit = 10;
    private int n_elem = 0;
 
    public BlockingQueue(int limit) {
        this.limit = limit;
    }  
    
    public synchronized void put(Object E) throws InterruptedException {
        while (this.queue.size() == this.limit) {
            wait();
        }
        if (this.queue.size() == 0) {
            notifyAll();
        }
        this.queue.add(E);
        this.n_elem += 1;
    }

    public synchronized Object take() throws InterruptedException {
        while (this.queue.size() == 0) {
            wait();
        }
        if (this.queue.size() == this.limit) {
            notifyAll();
        }
        this.n_elem -= 1
        return this.queue.remove(0);
    }

    public int remainingCapacity(){
        return this.limit - this.n_elem;
    }
}
```

## 2
```C
void *request (void *args) {
    int random_number = (rand() % 100) + 1; // Obtain a number between [1 - 100].
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
// basic node structure  
typedef struct __node_t {  
    int key;  
    struct __node_t *next;
 } node_t;

// basic list structure (one used per list)
typedef struct __list_t {
    node_t *head;
    pthread_mutex_t lock; // <- correção de problema
} list_t;

void List_Init(list_t *L) {
    L->head = NULL
    pthread_mutex_init(&L->lock, NULL) // <- correção de problema
}

int List_Insert(list_t *L, int key) {
    node_t* new = malloc(sizeof(node_t));
    if (new == NULL) {
        perror("malloc");
        return -1; // fail
    }
    new->key = key;
    
    pthread_mutex_lock(&L->lock); // <- correção de problema
    new->next = L->head;
    L->head = new;
    pthread_mutex_unlock(&L->lock); // <- correção de problema
    
    return 0; // success
}

int List_Lookup(list_t*L, int key) {
    pthread_mutex_lock(&L->lock); // <- correção de problema
    node_t*curr = L->head;
    while (curr) {
        if (curr->key == key){
            return 0; // success
        }
        curr =curr->next;
    }
    pthread_mutex_unlock(&L->lock); // <- correção de problema
    return -1; // failure
} 
```