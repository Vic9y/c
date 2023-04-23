# c

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
