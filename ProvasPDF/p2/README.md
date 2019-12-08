# Respostas
## 1

```Java
class CyclicBarrier {
    int numThreads;
    int count;

    public CyclicBarrier (int numThreads) {
        this.numThreads = numThreads;
        this.count = numThreads;
    }

    public synchronized void await () {
        this.count--;

        if(this.count > 0){
            this.wait();
        }
        else {
            this.count  = numThreads;
            notifyAll();
        }
    }
}

```

## 2

```C
typedef struct _shylock_t {
    int max_threads;
    int count_threads;
    pthread_mutex_t s_lock;
} shylock_t;

void shylock_init (shylock_t *lock, int max_nthreads) {
    lock-> max_threads = max_nthreads;
    lock-> count_threads = -1;
    pthread_mutex_init(&lock -> s_lock, NULL);
}

void shylock_acquire (shylock_t *lock) {
    if(lock.count_threads >= lock.max_threads){
        pthread_mutex_lock(&lock->s_lock);
    }
    else {
        lock->count_threads++;
    }
}

void shylock_release (shylock_t *lock) {
    if(lock.count_threads > 0) {
        pthread_mutex_unlock(&lock->s_lock);     
        lock-> count_threads--;
    }
}
```

## 3

```C
// basic node structure  
typedef struct __node_t {
    int value;
    struct __node_t *next;
} node_t;

typedef struct __queue_t {
    node_t *head;
    node_t *tail;
    pthread_mutex_t headLock; // <- Correção feita
    pthread_mutex_t tailLock; // <- Correção feita
} list_t;

void Queue_Init(queue_t *q) {
    note_t *tmp = malloc(sizeof(node_t));
    tmp->next = NULL;
    q->head = q->tail = tmp;
    Pthread_mutex_init(&q->headLock, NULL); // <- Correção feita
    Pthread_mutex_init(&q->tailLock, NULL); // <- Correção feita
}

void Queue_Enqueue(queue_t *q, int value) {
    node_t *tmp = malloc(sizeof(node_t));
    assert(tmp != NULL);
    tmp->value = value;
    tmp->next = NULL;

    pthread_mutex_lock(&q->tailLock); // <- Correção feita
    q->tail->next = tmp;
    q->tail = tmp;
    Pthread_mutex_unlock(&q->tailLock); // <- Correção feita
}

int Queue_Dequeue(queue_t *q, int *value) {
    pthread_mutex_lock(&q->headLock); // <- Correção feita
    node_t *tmp = q->head;
    node_t *new_head = tmp->next;
    if (new_head == NULL) {
        Pthread_mutex_unlock(&q->headLock); // <- Correção feita
        return -1; // queue was empty
    }
    *value = new_head->value;
    q->head = new_head;
    pthread_mutex_unlock(&q->headLock); // <- Correção feita
    
    free(tmp);
    return 0;
} 
```