

## Write a program to implement CPU Scheduling algorithms like FCFS & SJF

```
#include <stdio.h>

int main() {
    int n, i;
    float avg_waiting_time = 0, avg_turnaround_time = 0;
    printf("Enter the number of processes: ");
    scanf("%d", &n);

    int at[n], bt[n], wt[n], tat[n];
    printf("Enter the arrival time and burst time of each process:\n");
    for(i = 0; i < n; i++) {
        printf("Process %d: ", i+1);
        scanf("%d %d", &at[i], &bt[i]);
    }

    wt[0] = 0;
    tat[0] = bt[0];
    for(i = 1; i < n; i++) {
        wt[i] = tat[i-1] - at[i];
        if(wt[i] < 0)
            wt[i] = 0;
        tat[i] = bt[i] + wt[i];
    }

    printf("\nFCFS Scheduling:\n");
    printf("Process\tArrival Time\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for(i = 0; i < n; i++) {
        printf("%d\t%d\t\t%d\t\t%d\t\t%d\n", i+1, at[i], bt[i], wt[i], tat[i]);
        avg_waiting_time += wt[i];
        avg_turnaround_time += tat[i];
    }
    printf("Average waiting time = %.2f\n", avg_waiting_time/n);
    printf("Average turnaround time = %.2f\n", avg_turnaround_time/n);

    return 0;
}

```

```
#include <stdio.h>

int main() {
    int n, i, j, temp;
    float avg_waiting_time = 0, avg_turnaround_time = 0;
    printf("Enter the number of processes: ");
    scanf("%d", &n);

    int at[n], bt[n], wt[n], tat[n];
    printf("Enter the arrival time and burst time of each process:\n");
    for(i = 0; i < n; i++)
        scanf("%d %d", &at[i], &bt[i]);

    for(i = 0; i < n-1; i++) {
        for(j = i+1; j < n; j++) {
            if(bt[i] > bt[j]) {
                temp = bt[i];
                bt[i] = bt[j];
                bt[j] = temp;

                temp = at[i];
                at[i] = at[j];
                at[j] = temp;
            }
        }
    }

    wt[0] = 0;
    tat[0] = bt[0];
    for(i = 1; i < n; i++) {
        wt[i] = tat[i-1] - at[i];
        if(wt[i] < 0)
            wt[i] = 0;
        tat[i] = bt[i] + wt[i];
    }

    printf("\nSJF Scheduling:\n");
    printf("Process\tArrival Time\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for(i = 0; i < n; i++) {
        printf("%d\t%d\t\t%d\t\t%d\t\t%d\n", i+1, at[i], bt[i], wt[i], tat[i]);
        avg_waiting_time += wt[i];
        avg_turnaround_time += tat[i];
    }
    printf("Average waiting time = %.2f\n", avg_waiting_time/n);
    printf("Average turnaround time = %.2f\n", avg_turnaround_time/n);

    return 0;
}

```



##  Program to simulate producer and consumer problem using semaphores


```
#include <stdio.h>
#include <stdlib.h>

int mutex = 1, full = 0, empty = 3, x = 0;

void producer();
void consumer();
void wait(int*);
void signal(int*);

int main()
{
    int n;
    printf("\n1.Producer\n2.Consumer\n3.Exit");
    while (1)
    {
        printf("\nEnter your choice:");
        scanf("%d", &n);
        switch (n)
        {
        case 1:
            if ((mutex == 1) && (empty != 0))
                producer();
            else
                printf("Buffer is full!!");
            break;
        case 2:
            if ((mutex == 1) && (full != 0))
                consumer();
            else
                printf("Buffer is empty!!");
            break;
        case 3:
            exit(0);
            break;
        }
    }

    return 0;
}

void wait(int* s)
{
    (*s)--;
}

void signal(int* s)
{
    (*s)++;
}

void producer()
{
    wait(&mutex);
    signal(&full);
    wait(&empty);
    x++;
    printf("\nProducer produces the item %d", x);
    signal(&mutex);
}

void consumer()
{
    wait(&mutex);
    wait(&full);
    signal(&empty);
    printf("\nConsumer consumes item %d", x);
    x--;
    signal(&mutex);
}


```

## Write a program to demonstrate the concept of deadlock avoidance through Banker’s
Algorithm

```
#include<stdio.h>
int main()
{
int n , m , i , j , k;
n = 5;
m = 3;
int alloc[ 5 ] [ 3 ] = { { 0 , 1 , 0 },
{ 2 , 0 , 0 } ,
{ 3 , 0 , 2 } ,
{ 2 , 1 , 1 } ,
{ 0 , 0 , 2 } } ;
int max[ 5 ] [ 3 ] = { { 7 , 5 , 3 } ,
{ 3 , 2 , 2 } ,
{ 9 , 0 , 2 } ,
{ 2 , 2 , 2 } ,
{ 4 , 3 , 3 } } ;
int avail[3] = { 3 , 3 , 2 } ;
int f[n] , ans[n] , ind = 0 ;
for (k = 0; k < n; k++) {
f[k] = 0;
}
int need[n][m];
for (i = 0; i < n; i++) {
for (j = 0; j < m; j++)
need[i][j] = max[i][j] - alloc[i][j] ;
}
int y = 0;
for (k = 0; k < 5; k++){
for (i = 0; i < n; i++){
if (f[i] == 0){
int flag = 0;
for (j = 0; j < m; j++) {
if(need[i][j] > avail[j]){
flag = 1;
break;
}
}
if ( flag == 0 ) {
ans[ind++] = i;
for (y = 0; y < m; y++)
avail[y] += alloc[i][y] ;
f[i] = 1;
}
}
}
}
int flag = 1;
for(int i=0;i<n;i++)
{
if(f[i] == 0)
{
flag = 0;
printf(" The following system is not safe ");
break;
}
}
if (flag == 1)
{
printf(" Following is the SAFE Sequence \n ");
for (i = 0; i < n - 1; i++)
printf(" P%d -> " , ans[i]);
printf(" P%d ", ans[n - 1]);
}
return(0);
}
```


##  Write a program to implement dynamic partitioning placement algorithms i.e Best Fit, First
–Fit and Worst –Fit.


```
#include<stdio.h>
 
int main()
{
    int i, j, n, m, bsize[100], psize[100], bstatus[100], pstatus[100], fit, flag;
 
    printf("Enter number of memory blocks : ");
    scanf("%d",&n);
 
    printf("Enter the size of each block : ");
    for(i=0;i<n;i++)
    {
        scanf("%d",&bsize[i]);
        bstatus[i] = 0;
    }
 
    printf("Enter number of processes : ");
    scanf("%d",&m);
 
    printf("Enter the size of each process : ");
    for(i=0;i<m;i++)
    {
        scanf("%d",&psize[i]);
        pstatus[i] = 0;
    }
 
    printf("Memory Allocation Algorithm: \n");
    printf("1. First Fit\n");
    printf("2. Best Fit\n");
    printf("3. Worst Fit\n");
    printf("Enter your choice : ");
    scanf("%d",&fit);
 
    switch(fit)
    {
        case 1: // First Fit
            for(i=0;i<m;i++)
            {
                flag = 0;
                for(j=0;j<n;j++)
                {
                    if(bstatus[j] == 0 && bsize[j] >= psize[i])
                    {
                        bstatus[j] = 1;
                        pstatus[i] = 1;
                        flag = 1;
                        break;
                    }
                }
                if(flag == 0)
                    printf("Process %d cannot be allocated memory\n", i+1);
            }
            break;
 
        case 2: // Best Fit
            for(i=0;i<m;i++)
            {
                int min = 9999, pos = -1;
                for(j=0;j<n;j++)
                {
                    if(bstatus[j] == 0 && bsize[j] >= psize[i] && bsize[j] < min)
                    {
                        min = bsize[j];
                        pos = j;
                    }
                }
                if(pos != -1)
                {
                    bstatus[pos] = 1;
                    pstatus[i] = 1;
                }
                else
                    printf("Process %d cannot be allocated memory\n", i+1);
            }
            break;
 
        case 3: // Worst Fit
            for(i=0;i<m;i++)
            {
                int max = -1, pos = -1;
                for(j=0;j<n;j++)
                {
                    if(bstatus[j] == 0 && bsize[j] >= psize[i] && bsize[j] > max)
                    {
                        max = bsize[j];
                        pos = j;
                    }
                }
                if(pos != -1)
                {
                    bstatus[pos] = 1;
                    pstatus[i] = 1;
                }
                else
                    printf("Process %d cannot be allocated memory\n", i+1);
            }
            break;
    }
 
    printf("Memory blocks status: ");
    for(i=0;i<n;i++)
        printf("%d ", bstatus[i]);
    printf("\n");
 
    printf("Process status: ");
    for(i=0;i<m;i++)
        printf("%d ", pstatus[i]);
    printf("\n");
 
    return 0;
}
```

```
#include <stdio.h>
#include <conio.h>
#define max 25

void main()
{
    int frag[max], b[max], f[max], i, j, nb, nf, temp, lowest;
    static int bf[max], ff[max];

    clrscr();

    printf("Enter the number of blocks: ");
    scanf("%d", &nb);
    printf("Enter the number of files: ");
    scanf("%d", &nf);

    printf("\nEnter the size of the blocks:\n");
    for (i = 1; i <= nb; i++) {
        printf("Block %d: ", i);
        scanf("%d", &b[i]);
        bf[i] = 0;
    }

    printf("\nEnter the size of the files:\n");
    for (i = 1; i <= nf; i++) {
        printf("File %d: ", i);
        scanf("%d", &f[i]);
    }

    for (i = 1; i <= nf; i++) {
        lowest = 10000;
        for (j = 1; j <= nb; j++) {
            if (bf[j] != 1) {
                temp = b[j] - f[i];
                if (temp >= 0 && lowest > temp) {
                    ff[i] = j;
                    lowest = temp;
                }
            }
        }
        frag[i] = lowest;
        bf[ff[i]] = 1;
    }

    printf("\nFile No\tFile Size\tBlock No\tBlock Size\tFragment");
    for (i = 1; i <= nf && ff[i] != 0; i++) {
        printf("\n%d\t\t%d\t\t%d\t\t%d\t\t%d", i, f[i], ff[i], b[ff[i]], frag[i]);
    }

    getch();
}

```

```
#include <stdio.h>
#include <stdlib.h>

#define max 25

int main()
{
    int frag[max], b[max], f[max], i, j, nb, nf, temp, highest = 0;
    static int bf[max], ff[max];

    system("clear");

    printf("\n\tMemory Management Scheme - Worst Fit");
    printf("\nEnter the number of blocks:");
    scanf("%d", &nb);
    printf("Enter the number of files:");
    scanf("%d", &nf);
    printf("\nEnter the size of the blocks:-\n");
    for (i = 1; i <= nb; i++)
    {
        printf("Block %d:", i);
        scanf("%d", &b[i]);
    }
    printf("Enter the size of the files :-\n");
    for (i = 1; i <= nf; i++)
    {
        printf("File %d:", i);
        scanf("%d", &f[i]);
    }
    for (i = 1; i <= nf; i++)
    {
        for (j = 1; j <= nb; j++)
        {
            if (bf[j] != 1) //if bf[j] is not allocated
            {
                temp = b[j] - f[i];
                if (temp >= 0)
                    if (highest < temp)
                    {
                        ff[i] = j;
                        highest = temp;
                    }
            }
        }
        frag[i] = highest;
        bf[ff[i]] = 1;
        highest = 0;
    }
    printf("\nFile_no:\tFile_size :\tBlock_no:\tBlock_size:\tFragement");
    for (i = 1; i <= nf; i++)
        printf("\n%d\t\t%d\t\t%d\t\t%d\t\t%d", i, f[i], ff[i], b[ff[i]], frag[i]);

    return 0;
}

```

```
/*Program to implement First-Fit dynamic partitioning placement algorithm*/
#include <stdio.h>
#include <conio.h>
#define max 25

void main()
{
    int frag[max], b[max], f[max], i, j, nb, nf, temp;
    static int bf[max], ff[max];
    clrscr();
    printf("\n\tMemory Management Scheme - First Fit");
    printf("\nEnter the number of blocks:");
    scanf("%d", &nb);
    printf("Enter the number of files:");
    scanf("%d", &nf);
    printf("\nEnter the size of the blocks:\n");
    for(i = 1; i <= nb; i++)
    {
        printf("Block %d:", i);
        scanf("%d", &b[i]);
    }
    printf("Enter the size of the files:\n");
    for(i = 1; i <= nf; i++)
    {
        printf("File %d:", i);
        scanf("%d", &f[i]);
    }
    for(i = 1; i <= nf; i++)
    {
        for(j = 1; j <= nb; j++)
        {
            if(bf[j] != 1) //if bf[j] is not allocated
            {
                temp = b[j] - f[i];
                if(temp >= 0)
                {
                    ff[i] = j;
                    break;
                }
            }
        }
        frag[i] = temp;
        bf[ff[i]] = 1;
    }
    printf("\nFile_no:\tFile_size:\tBlock_no:\tBlock_size:\tFragement");
    for(i = 1; i <= nf; i++)
        printf("\n%d\t\t%d\t\t%d\t\t%d\t\t%d", i, f[i], ff[i], b[ff[i]], frag[i]);
    getch();
}

```

##  Write a program to implement various page replacement policies.

```
#include<stdio.h>
 
int main()
{
    int frames[100], pages[100], n, m, i, j, k, flag, fault = 0;
 
    printf("Enter number of frames : ");
    scanf("%d",&n);
 
    printf("Enter number of pages : ");
    scanf("%d",&m);
 
    printf("Enter the reference string : ");
    for(i=0;i<m;i++)
        scanf("%d",&pages[i]);
 
    for(i=0;i<n;i++)
        frames[i] = -1;
 
    printf("Page Replacement Policy: \n");
    printf("1. FIFO\n");
    printf("2. LRU\n");
    printf("3. Optimal\n");
    printf("Enter your choice : ");
    scanf("%d",&k);
 
    switch(k)
    {
        case 1: // FIFO
            j = 0;
            for(i=0;i<m;i++)
            {
                flag = 0;
                for(k=0;k<n;k++)
                {
                    if(frames[k] == pages[i])
                    {
                        flag = 1;
                        break;
                    }
                }
                if(flag == 0)
                {
                    frames[j] = pages[i];
                    j = (j+1)%n;
                    fault++;
                }
            }
            break;
 
        case 2: // LRU
            for(i=0;i<m;i++)
            {
                flag = 0;
                for(j=0;j<n;j++)
                {
                    if(frames[j] == pages[i])
                    {
                        flag = 1;
                        break;
                    }
                }
                if(flag == 0)
                {
                    int min = 9999, pos;
                    for(j=0;j<n;j++)
                    {
                        if(min > frames[j])
                        {
                            min = frames[j];
                            pos = j;
                        }
                    }
                    frames[pos] = pages[i];
                    fault++;
                }
            }
            break;
 
        case 3: // Optimal
            for(i=0;i<m;i++)
            {
                flag = 0;
                for(j=0;j<n;j++)
                {
                    if(frames[j] == pages[i])
                    {
                        flag = 1;
                        break;
                    }
                }
                if(flag == 0)
                {
                    int max = -1, pos;
                    for(j=0;j<n;j++)
                    {
                        int dist = 0;
                        for(k=i+1;k<m;k++)
                        {
                            if(frames[j] == pages[k])
                                break;
                            else
                                dist++;
                        }
                        if(dist > max)
                        {
                            max = dist;
                            pos = j;
                        }
                    }
                    frames[pos] = pages[i];
                    fault++;
                }
            }
            break;
    }
 
    printf("Number of page faults = %d\n",fault);
    return 0;
}
```

## Write a program to implement Disk Scheduling algorithms like FCFS, SCAN and C-SCAN.

FCFS:

```
#include<stdio.h>
#include<stdlib.h>
 
int main()
{
    int queue[20], n, head, i, seek = 0;
    float avg;
 
    printf("Enter the size of Queue : ");
    scanf("%d", &n);
 
    printf("Enter the Queue : ");
    for(i=1;i<=n;i++)
        scanf("%d",&queue[i]);
 
    printf("Enter the initial head position : ");
    scanf("%d", &head);
 
    queue[0] = head;
 
    for(i=0;i<n;i++)
    {
        seek += abs(queue[i+1] - queue[i]);
        printf("Disk head moves from %d to %d with seek %d\n",queue[i],queue[i+1],abs(queue[i+1] - queue[i]));
    }
 
    printf("Total seek time = %d\n", seek);
    avg = seek/(float)n;
    printf("Average seek time = %f\n", avg);
 
    return 0;
}
```
SCAN:
```
#include<stdio.h>
#include<stdlib.h>
 
int main()
{
    int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], queue2[20], temp1 = 0, temp2 = 0;
    float avg;
 
    printf("Enter the size of Queue : ");
    scanf("%d", &n);
 
    printf("Enter the Queue : ");
    for(i=1;i<=n;i++)
        scanf("%d",&queue[i]);
 
    printf("Enter the initial head position : ");
    scanf("%d", &head);
 
    queue[0] = head;
 
    for(i=0;i<n;i++)
    {
        max = 0;
        for(j=i+1;j<=n;j++)
        {
            if(queue[i]>=queue[j])
                diff = queue[i]-queue[j];
            else
                diff = queue[j]-queue[i];
            if(diff>max)
            {
                max = diff;
                temp = j;
            }
        }
        queue1[temp1] = queue[temp];
        temp1++;
        for(k=temp;k<=n-1;k++)
        {
            queue[k] = queue[k+1];
        }
        n--;
    }
 
    queue2[temp2] = head;
    for(i=0;i<temp1-1;i++)
    {
        queue2[i+1] = queue1[i];
    }
 
    for(i=0;i<temp1;i++)
    {
        seek += abs(queue2[i+1] - queue2[i]);
        printf("Disk head moves from %d to %d with seek %d\n",queue2[i],queue2[i+1],abs(queue2[i+1] - queue2[i]));
    }
 
    printf("Total seek time = %d\n", seek);
    avg = seek/(float)temp1;
    printf("Average seek time = %f\n", avg);
 
    return 0;
}
```
C-SCAN :

```

#include<stdio.h>
#include<stdlib.h>
 
int main()
{
    int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], queue2[20], temp1 = 0, temp2 = 0;
    float avg;
 
    printf("Enter the size of Queue : ");
    scanf("%d", &n);
 
    printf("Enter the Queue : ");
    for(i=1;i<=n;i++)
        scanf("%d",&queue[i]);
 
    printf("Enter the initial head position : ");
    scanf("%d", &head);
 
    queue[0] = head;
 
    for(i=0;i<=n;i++)
    {
        for(j=i+1;j<=n;j++)
        {
            if(queue[i]>queue[j])
            {
                temp = queue[i];
                queue[i] = queue[j];
                queue[j] = temp;
            }
        }
    }
 
    max = queue[n];
 
    for(i=head;i<=max;i++)
    {
        queue1[temp1] = queue[i];
        temp1++;
    }
 
    queue1[temp1] = max;
    temp1++;
 
    for(i=max-1;i>=0;i--)
    {
        queue2[temp2] = queue[i];
        temp2++;
    }
 
    queue2[temp2] = 0;
    temp2++;
 
    seek = head + max;
 
    for(i=0;i<temp1-1;i++)
    {
        seek += abs(queue1[i+1] - queue1[i]);
        printf("Disk head moves from %d to %d with seek %d\n",queue1[i],queue1[i+1],abs(queue1[i+1] - queue1[i]));
    }
 
    for(i=0;i<temp2-1;i++)
    {
        seek += abs(queue2[i+1] - queue2[i]);
        printf("Disk head moves from %d to %d with seek %d\n",queue2[i],queue2[i+1],abs(queue2[i+1] - queue2[i]));
    }
 
    printf("Total seek time = %d\n", seek);
    avg = seek/(float)(temp1+temp2-2);
    printf("Average seek time = %f\n", avg);
 
    return 0;
}
```




