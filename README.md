- 👋 Hi, I’m @minthukhaing
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
minthukhaing/minthukhaing is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
Само сложение заключается в коротком цикле:
For i:=1 to N do    // Цикл по количеству разрядов
  Begin
   Sum[i]:=(A[i]+B[i]+carry) mod Radix[i];        // Сумма разрядов
  carry:=(A[i]+B[i]+carry) div Radix[i];         // перенос в следующий разряд
  End;

Обращаю внимание, что в полях ввода все числа записываются (через пробел) младшими разрядами вперёд!
------------------------------------------------------------------
mpi and find pi value
#include<stdio.h>
#include<mpi.h>
int main(int argc,char *argv[])
{
    int myid,np,i,j;
    int tag=666;
    double pi=0.0; 
    double fVal;// fval represents the function value of 4 / (1 + x ^ 2) corresponding to the XI, that is, the height of each rectangle
    int n=100;/ / Change the value of N, indicating that the number of differentiated small rectangles is changed, the more it is more close to extreme thinking.
    MPI_Status status;
    double h=(double)1/n; / / Width of each rectangle
    double local=0.0;/ / The area calculated for each process and 
    double start,end;
    double a;
    MPI_Init(&argc,&argv);// Start the parallel program    
    MPI_Comm_size(MPI_COMM_WORLD,&np);/ / Get the total number of processes, NP represents the number of processes, can be set through -np at runtime
    MPI_Comm_rank(MPI_COMM_WORLD,&myid);/ / Get the current process number
    start=MPI_Wtime();// Record start time
    for(i=myid;i<n;i+=np) // Calculate the rectangular area of ​​each part by using NP processes
    {
            a=(i+0.5)*h;
            fVal=4.0/(1.0+a*a);// Get f (xi) 
            local+=fVal ; 
    }
    local=local*h;// Get the area calculated by this process 
 
    // process number! The result of the process calculation of = 0 is sent to the process 0 
    if(myid!=0)
   {   
        MPI_Send(&local,1,MPI_DOUBLE,0,myid,MPI_COMM_WORLD); 
    }
    if(myid==0) // The process number is 0, accumulating the calculation results of each process 
    {
        pi=local;// Receive this value after receiving the value of the process 0 
        for(j=1;j<np;j++)
           {
                MPI_Recv(&local,1,MPI_DOUBLE,j,j,MPI_COMM_WORLD,&status); // Send the results of other processes to local 
                pi+=local;// Get all the area and 
           }
    }
    end=MPI_Wtime();// End calculation time 
    if(myid==0)
        {
          printf("PI = %.15f\n",pi);
          printf("Time = %lf\n",end-start); 
          printf("np = %d\n",np);
          }
    MPI_Finalize();
    return 0;
}-----------------------------------------------]
#include <mpi.h>
#include <iostream>
#include <stdio.h>
#include<time.h>
#include<math.h>
using namespace std;
int main(int argc, char** argv)
{
	int rank,size;
	double x, y;
	double start_time, end_time;
	double my_pi, Real_pi, E_computing;
	int Local_point;
	int	N_of_point = 5;
	double N_of_point_Circle = 0, Radius;
	MPI_Init(&argc, &argv);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	MPI_Comm_size(MPI_COMM_WORLD, &size);
	if (rank == 0)
	{
		start_time = MPI_Wtime();
		Local_point = N_of_point / size;
	}
	MPI_Bcast(&N_of_point, 1, MPI_INT, 0, MPI_COMM_WORLD);
	time_t ti;
	srand((unsigned)time(&ti));
	for (int i=1; i < Local_point; i++)
	{
		x = rand() / (RAND_MAX + 1.0);
		y = rand() / (RAND_MAX + 1.0);
		//std::cout << "x-->\t" << x << "\t" << "y-->\t" << y << endl;
		Radius = sqrt(x * x + y * y);
		if (Radius <= 1)
			N_of_point_Circle++;
	}
	MPI_Reduce (&Local_point, &N_of_point_Circle ,1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
	if (rank == 0)
	{
		Real_pi = 3.14159265358932385;
		my_pi = (double)(4 * N_of_point_Circle) / Local_point;
		E_computing = fabs(Real_pi-my_pi);
		end_time = MPI_Wtime();
		std::cout << "Real_PI-->\t" << Real_pi << endl;
		std::cout << "MY_PI-->\t" << my_pi << endl;
		std::cout << "Eroor of computing--->\t" << E_computing << endl;
		std::cout << "Time-->\t" << end_time - start_time << endl;
	}
	MPI_Finalize();
	return 0;
}
