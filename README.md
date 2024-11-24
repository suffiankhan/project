
#include<stdio.h>
#include<string.h>

#define totalfloors 4
#define roomsonfloor 4

int checkUser(char name[], char password[]){
    FILE *users = fopen("records.txt", "r");

    if(users == NULL){
        printf("Error Occured");
    }
    else{
        char name_in[20];
        char pass_in[8];
        char unused;

        while(fgets(name_in, sizeof(name_in), users) != NULL ){
        	
        	name_in[strcspn(name_in, "\n")] = '\0';  // replaces \n with \0 to mark the end of name
            fgets(pass_in, sizeof(pass_in)+1, users);
            pass_in[strcspn(pass_in, "\n")] = '\0'; // replaces \n with \0 to mark the end of password
            
            if(strcmp(name, name_in)==0){
                if(strcmp(password, pass_in)==0){
                    printf("Logged in successfully!\n");
                    fclose(users);
                    return 1;
                }else{
                    printf("Incorrect Password\n");
					return 0;
                }
            }
        }
		printf("User not found\n");
		fclose(users);
		return 0;
    }
};

int createUser(char name[], char password[]){
	FILE *users = fopen("records.txt", "a");
	char file_name[30];
	if(users == NULL){
		printf("Error Occured\n");
		return 0;
	}else{
		fputs(name, users);
		fputs(password, users);
		name[strcspn(name, "\n")] = '\0';
		password[strcspn(password, "\n")] = '\0';
		strcpy(file_name, name);
		strcat(file_name, password);
		strcat(file_name, ".txt");
		printf("%s", file_name);
		FILE *user_file = fopen(file_name, "w");
		if(user_file == NULL){
			printf("Error Occured!");
			return 0;
		}else{
			fclose(user_file);
		}
		return 1;
	}
}

void availableRooms(){
	FILE *rooms = fopen("roombooking.txt", "r");
	if(rooms == NULL){
		printf("Error Occured\n");
	}else{
		char room;
		char type[4][10] = {"Deluxe", "Premium", "Standard", "Shared"};
		int floor_count = 0; 
		int room_count = 0;
		while(fscanf(rooms, "%c", &room) != EOF){
			if(room == '0'){
				printf("%s Room %d\n", type[floor_count], room_count+1);
			}
			room_count++;
			if(room == '\n'){
				room_count = 0;
				floor_count++;
			}
		}
		fclose(rooms);
	}
};

void bookRoom(int floor, int room, char name[], char password[]){
	char rooms_arr[totalfloors][roomsonfloor+1];
	char file_name[30];

	FILE *rooms = fopen("roombooking.txt", "r");
	if(rooms == NULL){
		printf("Error Occured!");
	}else{
		for (int i = 0; i < 4; i++) {
            for (int j = 0; j < 5; j++) {
                fscanf(rooms, "%c", &rooms_arr[i][j]);
            }
        }
        fclose(rooms);
		rooms_arr[floor][room] = '1';
	}
	FILE *rooms_new = fopen("roombooking.txt", "w");
	if(rooms == NULL){
		printf("Error Occured!");
	}else{
		for (int i = 0; i < 4; i++) {
            for (int j = 0; j < 5; j++) {
                fprintf(rooms_new, "%c", rooms_arr[i][j]);
            }
        }
        fclose(rooms);
	}
	strcpy(file_name, name);
	// strcat(file_name, password);
	strcat(file_name, ".txt");
	FILE *user_file = fopen(file_name, "a");
	if(user_file == NULL){
		printf("Error Occured!");
	}else{
		fprintf(user_file,"%d\n%d\n", floor+1, room+1);
		fclose(user_file);
	}

};
void foododering(char name[])
{
	char file_name[30];
	int foodbooking=0;
	int numberofdays=0;
	int foodtotal=0;
	printf("------------------------------------------------\n");
	printf("             We have three deal of food:        \n");
	printf("------------------------------------------------\n");

	printf("Deal 1: price 50000\n");
	printf("1 -> Veg Manchurian\n");
	printf("2 -> Mutton Fry\n");
	printf("4 -> Shawarma Roll\n");
	printf("5 -> Fish Fry\n");

	printf("Deal 2: price 60000\n");
	printf("1 -> Handi\n");
	printf("2 -> Malai Boti\n");
	printf("4 -> Kabab\n");
	printf("5 -> Prawns\n");

	printf("Deal 3: price 80000\n");
	printf("1 -> Handi(Makhni)\n");
	printf("2 -> Mutton Boti Fry\n");
	printf("4 -> Ginger Chicken\n");
	printf("5 -> Mushroom Gravy\n");

	printf("press one for deal one and +1 for other:");
	scanf("%d",&foodbooking);

	printf("how many days of food you want:");
	scanf("%d",&numberofdays);

	if(foodbooking==1){
		foodtotal=numberofdays*50000;
	}
	else if(foodbooking==2){
		foodtotal=numberofdays*60000;
	}
	else{
		foodtotal=numberofdays*80000;
	}
	strcpy(file_name, name);
	// strcat(file_name, password);
	strcat(file_name, ".txt");
	FILE *user_file = fopen(file_name, "a");
	if(user_file == NULL){
		printf("Error Occured!");
	}else{
		fprintf(user_file,"price of food for %d days:%d",numberofdays, foodtotal);
		fclose(user_file);
	}

}
int main(){
    char name[20]; 
    char pass[8];
    int choice=0;
    int floor, room;
    printf("------------------------------------------------\n");
    printf("		WELCOME TO KHAN HOTEL           \n");
    printf("		Select an Option                \n");
    printf("		1. Login                        \n");
    printf("		2. Create Account               \n");
    printf("		3. Exit                         \n");
    printf("------------------------------------------------\n");
    scanf("%d", &choice);
    
    switch(choice){
    	case 1:
    		do{
        		printf("Enter username: ");
        		scanf("%s", &name);
        		printf("Enter Password: ");
        		scanf("%s", &pass);
    		}while(checkUser(name, pass) == 0);
    		break;
    	case 2:
    		do{
        		printf("Enter username: ");
        		scanf("%s", &name);
        		printf("Enter Password: ");
        		scanf("%s", &pass);
        		strcat(name, "\n");
        		strcat(pass, "\n");
    		}while(createUser(name, pass) == 0);
    		break;
    
		case 3:
    		return 0;
	}
	while(choice!=3){
	
		printf("------------------------------------------------\n");
		printf("		1. Check Available Rooms                        \n");
	    printf("		2. Book A Room               \n");
	    printf("		3. Exit                         \n");
	    printf("------------------------------------------------\n");
		scanf("%d", &choice);
		
		switch(choice){
			case 1:
				printf("--------------------------------------\n");
				printf("          Available Rooms\n");
				printf("--------------------------------------\n");
				availableRooms();
				break;
			case 2:
				printf("------------------------------------------------\n");
				printf("		1. Delux Room                        \n");
	    		printf("		2. Premium Room               \n");
	    		printf("		3. First Class                         \n");
	    		printf("		4. Shared                         \n");
				printf("------------------------------------------------\n");
				scanf("%d", &floor);
				printf("Enter Room Number from available rooms of this category: ");
				scanf("%d", &room);
				bookRoom(floor-1, room-1, name, pass);
			case 4:
			foododering(name);
			break;	
		}
		printf("Press 3 to exit and 4 for food or any other number to continue");
		scanf("%d", &choice);
	}
	return 0;
    
}
