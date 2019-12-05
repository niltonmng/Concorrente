# Respostas
## 1

## 2
```
#include <stdio.h>
#include <assert.h>
#include <pthread.h>
#include <time.h>

int numberOfThreads = 5;

void *request (void *args) {
    int random_number = (rand() % 30) + 1; // // Obtain a number between [1 - 30].
    printf("Request will sleep %d seconds\n", random_number);
    sleep(random_number); // Sleeps seconds
    pthread_exit(random_number);
}

int gateway (int num_replicas) {
    pthread_t pthreads[num_replicas];
    for (int i = 0; i < num_replicas; i++) {
        pthread_create (&pthreads[i], NULL, &request, (void*) i);
    }
    
    int sum = 0;
    printf("Joiner waiting...\n");
    for (int i = 0; i < num_replicas; i++) {
        void* aux;
        pthread_join(pthreads[i], &aux);    
        printf("Thread %d finished. It slept %d seconds.\n", i, (int) aux);
        sum += (int) aux;
    }
    return sum;
}

int main (int argc, char *argv[]) {
    srand ( time(NULL) );
    printf("gateway=%d\n", gateway(numberOfThreads));
}
```

## 3