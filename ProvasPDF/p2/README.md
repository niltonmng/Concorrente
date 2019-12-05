# Respostas
## 1

```
```

## 2

```
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

void Queue_Init(queue_t *Q) {
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

    Pthread_mutex_lock(&q->tailLock); // <- Correção feita
    q->tail->next = tmp;
    q->tail = tmp;
    Pthread_mutex_unlock(&q->tailLock); // <- Correção feita
}

int Queue_Dequeue(queue_t *q, int *value) {
    Pthread_mutex_lock(&q->headLock); // <- Correção feita
    node_t *tmp = q->head;
    node_t *new_head = tmp->next;
    if (new_head == NULL) {
        Pthread_mutex_unlock(&q->headLock); // <- Correção feita
        return -1; // queue was empty
    }
    *value = new_head->value;
    q->head = new_head;
    Pthread_mutex_unlock(&q->headLock); // <- Correção feita
    
    free(tmp);
    return 0;
} 
```