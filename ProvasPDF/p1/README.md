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