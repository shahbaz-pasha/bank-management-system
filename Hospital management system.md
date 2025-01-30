```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<string.h>

struct patient{
    int id;
    int age;
    char patientName[50];
    char gender[2];
    char patientAddress[50];
    char disease[50];
    char date[12];
}p;

struct doctor{
    int id;
    char name[50];
    char address[50];
    char specialize[50];
    char date[12];
}d;

struct Appointment {
    int slot;
    struct patient Patient;
    char doctor[50];
};a;

FILE *fp;

int main(){
    struct Appointment appointments[10] = {0};
    int appointmentCount = 0;

    int ch;

    while(1){
        system("cls");
        printf("<== Hospital Management System ==>\n");
        printf("\n");
        printf("1.Admit Patient\n");
        printf("2.Patient List\n");
        printf("3.Discharge Patient\n");
        printf("4.Add Doctor\n");
        printf("5.Doctors List\n");
        printf("6.Schedule an Appointment\n");
        printf("7.Display Appointments\n");
        printf("0.Exit\n\n");
        printf("Enter your choice: ");
        scanf("%d", &ch);

        switch(ch){
        case 0:
            exit(0);

        case 1:
            admitPatient();
            break;

        case 2:
            patientList();
            break;

        case 3:
            dischargePatient();
            break;

        case 4:
            addDoctor();
            break;

        case 5:
            doctorList();
            break;

        case 6:
            scheduleAppointment(appointments, &appointmentCount);
            break;

        case 7:
            displayAppointment(appointments, appointmentCount);
            break;

        default:
            printf("Invalid Choice...\n\n");

        }
        printf("\n\nPress Any Key To Continue...");
        getch();
    }

    return 0;
}

void admitPatient(){
    char myDate[12];
    time_t t = time(NULL);
    struct tm tm = *localtime(&t);
    sprintf(myDate, "%02d/%02d/%d", tm.tm_mday, tm.tm_mon+1, tm.tm_year + 1900);
    strcpy(p.date, myDate);

    fp = fopen("Newpatient.txt", "ab");

    printf("Enter Patient id: ");
    scanf("%d", &p.id);

    printf("Enter Patient Age: ");
    scanf("%d", &p.age);

    printf("Enter Patient name: ");
    fflush(stdin);
    gets(p.patientName);

    printf("Enter Patient Gender: ");
    fflush(stdin);
    gets(p.gender);

    printf("Enter Patient Address: ");
    fflush(stdin);
    gets(p.patientAddress);

    printf("Enter Patient Disease: ");
    fflush(stdin);
    gets(p.disease);

    printf("\nPatient Added Successfully");

    fwrite(&p, sizeof(p), 1, fp);
    fclose(fp);
}

void patientList(){

    system("cls");
    printf("<== Patient List ==>\n\n");
    printf("%-10s %-10s %-20s %-20s %-20s %-20s %s\n", "Id", "Age", "Patient Name", "Gender", "Address", "Disease", "Date");
    //printf("%s\t %s\t %s\t\t %s\t\t\t %s\t %s\t %s\n", "Id", "Age", "Patient Name", "Gender", "Address", "Disease", "Date");
    printf("-----------------------------------------------------------------------------------------------------------------------\n");

    fp = fopen("Newpatient.txt", "rb");
    while(fread(&p, sizeof(p), 1, fp) == 1){
            //printf("%d\t %d\t %s\t\t %s\t\t\t %s\t %s\t %s\n", p.id, p.age, p.patientName, p.gender, p.patientAddress, p.disease, p.date);
        printf("%-10d %-10d %-20s %-20s %-20s %-20s %s\n", p.id, p.age, p.patientName, p.gender, p.patientAddress, p.disease, p.date);
    }

    fclose(fp);
}


void dischargePatient(){
    int id, f=0;
    system("cls");
    printf("<== Discharge Patient ==>\n\n");
    printf("Enter Patient id to discharge: ");
    scanf("%d", &id);

    FILE *ft;

    fp = fopen("Newpatient.txt", "rb");
    ft = fopen("temp.txt", "wb");

    while(fread(&p, sizeof(p), 1, fp) == 1){

        if(id == p.id){
            f=1;
        }else{
            fwrite(&p, sizeof(p), 1, ft);
        }
    }

    if(f==1){
        printf("\n\nPatient Discharged Successfully.");
    }else{
        printf("\n\nRecord Not Found !");
    }

    fclose(fp);
    fclose(ft);

    remove("Newpatient.txt");
    rename("temp.txt", "Newpatient.txt");

}

void addDoctor(){

    char myDate[12];
    time_t t = time(NULL);
    struct tm tm = *localtime(&t);
    sprintf(myDate, "%02d/%02d/%d", tm.tm_mday, tm.tm_mon+1, tm.tm_year + 1900);
    strcpy(d.date, myDate);

    int f=0;

    system("cls");
    printf("<== Add Doctor ==>\n\n");

    fp = fopen("Newdoctor.txt", "ab");

    printf("Enter Doctor id: ");
    scanf("%d", &d.id);

    printf("Enter Doctor Name: ");
    fflush(stdin);
    gets(d.name);

    printf("Enter Doctor Address: ");
    fflush(stdin);
    gets(d.address);

    printf("Doctor Specialize in: ");
    fflush(stdin);
    gets(d.specialize);

    printf("Doctor Added Successfully\n\n");

    fwrite(&d, sizeof(d), 1, fp);
    fclose(fp);
}



void doctorList(){
    system("cls");
    printf("<== Doctor List ==>\n\n");

    printf("%-10s %-30s %-30s %-30s %s\n", "id", "Name", "Address", "Specialize","Date");
    printf("-------------------------------------------------------------------------------------------------------------------\n");

    fp = fopen("Newdoctor.txt", "rb");
    while(fread(&d, sizeof(d), 1, fp) == 1){
        printf("%-10d %-30s %-30s %-30s %s\n", d.id, d.name, d.address, d.specialize, d.date);
    }

    fclose(fp);
}

void patientAppointment() {
    printf("Available Time Slots:\n");
    printf("1. 9:00 AM - 10:00 AM\n");
    printf("2. 10:00 AM - 11:00 AM\n");
    printf("3. 11:00 AM - 12:00 PM\n");
    printf("4. 1:00 PM - 2:00 PM\n");
    // Add more time slots as needed*/
}
void scheduleAppointment(struct Appointment appointments[], int *appointmentCount) {

    system("cls");
    if (*appointmentCount >= 10) {
        printf("Appointment slots are full.\n");
        return;
    }
 fp = fopen("appointment.txt", "rb");
    patientAppointment();

    int slot;
    printf("Enter preferred time slot (1-4): ");
    scanf("%d", &slot);
    if (slot < 1 || slot > 4) {
        printf("Invalid time slot.\n");
        return;
    }

    if (appointments[slot - 1].slot != 0) {
        printf("Slot already booked. Please choose another slot.\n");
        return;
    }

    printf("Enter patient name: ");
    scanf("%s", appointments[slot - 1].Patient.patientName);
    printf("Enter patient age: ");
    scanf("%d", &appointments[slot - 1].Patient.age);
    appointments[slot - 1].Patient.id = *appointmentCount + 1;
    strcpy(appointments[slot - 1].doctor, "Dr.Smith"); // Assuming one doctor for simplicity
    appointments[slot - 1].slot = slot;

    printf("Appointment scheduled successfully.\n");
    (*appointmentCount)++;
     fclose(fp);
}

void displayAppointment(struct Appointment appointments[], int appointmentCount) {
    int i;
    printf("Scheduled Appointments:\n");
    for (i = 0; i < 10; i++) {
        if (appointments[i].slot != 0) {
            printf("Slot %d: %s (%d years) - Doctor: %s\n",
                appointments[i].slot, appointments[i].Patient.patientName,
                appointments[i].Patient.age, appointments[i].doctor);
        }
    }
}

```
