# Read-csv-store-in-array-of-structures-and-manipulate-the-data

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<float.h>

typedef struct{
    char first_name[50];
    char last_name[50];
    char major[100];
    char degree[50];
    float gpa;
    int credit_hours;
    char ta[10];
    char advisor[100];
} Student;

void printValues(Student students[], int row_count)
{
    printf("First Name, Last Name, Major, Degree, GPA, Credit Hours, TA, Advisor\n");
    int i;
    for(i=0; i<row_count-1; i++)
    {
        printf("%s, %s, %s, %s, %.2f, %d, %s, %s\n", students[i].first_name, students[i].last_name, students[i].major, students[i].degree, students[i].gpa, students[i].credit_hours, students[i].ta, students[i].advisor);

    }

}

void printDegrees(Student students[], int row_count)
{
    printf("Solution 1 : ");
    int x=0;
    int i,j;
    for (i=0; i<row_count-1; i++)
    { 
        for (j=0; j<i; j++)
        {
            if(strcmp(students[i].degree,students[j].degree)==0)
            {
                break;
            }
        }
        if (i == j)
        {
            printf("\n%s  ",students[i].degree);
            x++;
        }
    }
    printf("\nHence, we have %d different degrees.\n\n", x);
}

void printHighgpa(Student students[], int row_count)
{
    printf("Solution 2 : \n");
    printf("Students who have the highest GPA are : \n");
    float first, second, third;
    third = first = second = FLT_MIN;
    int i;
    for (i = 0; i < row_count-1; i++)
    {
        if (students[i].gpa > first)
        {
            third = second;
            second = first;
            first = students[i].gpa;
        }
        else if (students[i].gpa > second)
        {
            third = second;
            second = students[i].gpa;
        }
        else if (students[i].gpa > third)
        {
            third = students[i].gpa;
        }
    }
    float a[3];
    a[0]=first;
    a[1]=second;
    a[2]=third;
    int x;

    for(int i=0;i<3;i++)
    {
        x=0;
        for(int j=0;j<row_count-1;j++)
        {
            if (students[j].gpa == a[i])
            {
                printf("%s %s --> %.2f\n", students[j].first_name, students[j].last_name, students[j].gpa);
                x++; 
            }
        }
        //if(x==3)
        //{
           // break;
        //}
    }
}

void printAvgCredithrs(Student students[], int row_count)
{
    printf("Solution 3 : \n");
    int sum=0;
    for(int i=0; i<row_count-1; i++)
    {
        sum=sum+students[i].credit_hours;
    }
    int avg=sum/(row_count-1);
    printf("The average credit hours of the college are %d hours.\n\n", avg); 
}

void printAvgCgpa(Student students[], int row_count)
{
    printf("Solution 4 : \n");
    char a[] = "Computer Science";
    float sum=0;
    int x=0;
    for(int i=0;i<row_count-1;i++)
    {
        if(strcmp(students[i].major,a)==0)
        {
            sum=sum+students[i].gpa;
            x++;
        }
    }
    float avg=sum/x;
    printf("The average GPA of the students whose department is Computer Science is %.2f.\n\n", avg);
}

void printDeptAdvisor(Student students[], int row_count)
{
    printf("Solution 5 : \n");
    printf("The list of departments along with the total number of advisors are as follows : \n");
    char a[100][100];
    char b[100][100];
    int j,k;
    int m=0;
    for(int i=0; i<row_count-1; i++)
    {
        for (j=0; j<i; j++)
        {
            if(strcmp(students[i].major,students[j].major)==0)
            {
                break;
            }
        }
        if (i == j)
        {
            strcpy(a[m],students[i].major);
            m++;
        }
    }

    int n=0;
    for(int i=0;i<row_count-1;i++)
    {
        for(k=0;k<i;k++)
        {
            if(strcmp(students[i].advisor,students[k].advisor)==0)
            {
                break;
            }
        }
        if(i==k)
        {
            strcpy(b[n],students[i].advisor);
            n++;
        }
    }

    int count;
    char c[100][100];
    char d = '\r';
    for(int s=0;s<m;s++)
    {
        count=0;
        for(int t=0;t<n;t++)
        {
            for(int u=0;u<row_count-1;u++)
            {
                if((strcmp(students[u].major,a[s])==0) && (strcmp(students[u].advisor,b[t])==0))
                {
                    count++;
                    break;
                }

            }
        }
        printf("%s --> %d\n", a[s],count);
    }

}
               
int main()
{
    FILE *myFile = fopen("students_dataset.csv", "r");

    if(myFile == NULL)
    {
        printf("File did not open successfully.");
    }
    char myLine[1024];
    int row_count=0;
    int field_count=0;

    Student students[1000];

    int i=0;
    while(fgets(myLine, 1024, myFile))
    {
        field_count=0;
        row_count++;
        if(row_count==1)
        {
            continue;
        }
        char *field = strtok(myLine, ",");
        while(field)
        {
            if(field_count==0)
            {
                strcpy(students[i].first_name, field);
            }
            if(field_count==1)
            {
                strcpy(students[i].last_name, field);
            }
            if(field_count==2)
            {
                strcpy(students[i].major, field);
            }
            if(field_count==3)
            {
                strcpy(students[i].degree, field);
            }
            if(field_count==4)
            {
                students[i].gpa = atof(field);
            }
            if(field_count==5)
            {
                students[i].credit_hours = atoi(field);
            }
            if(field_count==6)
            {
                strcpy(students[i].ta, field);
            }
            if(field_count==7)
            {
                strcpy(students[i].advisor, field);
            }
        
            field = strtok(NULL,",");
            field_count++;
        }
        i++;
    }
    fclose(myFile);

    printValues(students, row_count);
    
    printf("Q1. How many different degrees do we have? Display their names.\n");
    printDegrees(students, row_count);

    printf("---------------------------------------------------------------------------------------------------------------");

    printf("Q2. What is the full name of the 3 students who have the highest GPA? If we have multiple, print them all.\n");
    printHighgpa(students, row_count);

    printf("---------------------------------------------------------------------------------------------------------------");

    printf("\nQ3. What are the average credit hours of the college?\n");
    printAvgCredithrs(students, row_count);

    printf("---------------------------------------------------------------------------------------------------------------");

    printf("Q4. What is the average GPA of the students whose department is Computer Science?\n");
    printAvgCgpa(students, row_count);

    printf("---------------------------------------------------------------------------------------------------------------");

    printf("Q5. Display the list of departments along with the total number of advisors.\n");
    printDeptAdvisor(students, row_count);

    printf("---------------------------------------------------------------------------------------------------------------");

    return 0;
}


/* References:
1. https://www.youtube.com/watch?v=uUcDSZ8CFhg
2. https://www.geeksforgeeks.org/print-distinct-elements-given-integer-array/
3. https://www.geeksforgeeks.org/find-the-largest-three-elements-in-an-array/
4. https://www.geeksforgeeks.org/program-average-array-iterative-recursive/
5. https://www.geeksforgeeks.org/remove-all-occurrences-of-a-character-in-a-string/
6. https://stackoverflow.com/questions/2684603/how-do-i-initialize-a-float-to-its-max-min-value
*/
