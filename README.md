# AICP-Task-2
# Train management system
#include <iostream>
#include <vector>
#include <string>
using namespace std;

const int COACH_SEATS = 80;
const int NUM_COACHES = 6;
const int TICKET_PRICE = 25;
const int FREE_TICKET_THRESHOLD = 10;

struct TrainJourney {
    string departureTime;
    string returnTime;
    vector<int> availableSeats;
    int totalPassengers;
    int totalRevenue;
};

int main() 
{
    vector<TrainJourney> trainSchedule;

    TrainJourney train1{"09:00", "10:00", vector<int>(NUM_COACHES, COACH_SEATS), 0, 0};
    TrainJourney train2{"11:00", "12:00", vector<int>(NUM_COACHES, COACH_SEATS), 0, 0};
    TrainJourney train3{"13:00", "14:00", vector<int>(NUM_COACHES, COACH_SEATS), 0, 0};
    TrainJourney train4{"15:00", "16:00", vector<int>(NUM_COACHES, COACH_SEATS), 0, 0};

    trainSchedule.push_back(train1);
    trainSchedule.push_back(train2);
    trainSchedule.push_back(train3);
    trainSchedule.push_back(train4);

    cout << "Electric Mountain Railway - Start of the Day\n";

    while (true) 
	{
        cout << "\nTrain Schedule:\n";
        for (int i = 0; i < trainSchedule.size(); i++) 
		{
            TrainJourney& journey = trainSchedule[i];
            cout << "Train " << (i + 1) << ": " << journey.departureTime << " to " << journey.returnTime << " - Seats: ";
            if (journey.availableSeats[0] == 0) 
			{
                cout << "Closed\n";
            } 
			else 
			{
                cout << journey.availableSeats[0] << " remaining\n";
            }
        }

        int choice;
        cout << "\nEnter the train number to book tickets (1-4) or 0 to exit: ";
        cin >> choice;

        if (choice == 0) 
		{
            break;
        } 
		else if (choice >= 1 && choice <= 4) 
		{
            TrainJourney& selectedJourney = trainSchedule[choice - 1];
            int numPassengers;
            cout << "Enter the number of passengers: ";
            cin >> numPassengers;

            if (numPassengers < 1) 
			{
                cout << "Invalid number of passengers. Please try again.\n";
                continue;
            }

            int totalCost = TICKET_PRICE * numPassengers;
            int freeTickets = numPassengers / FREE_TICKET_THRESHOLD;

            if (freeTickets > 0) 
			{
                totalCost -= freeTickets * TICKET_PRICE;
            }

            if (totalCost < 0) 
			{
                totalCost = 0;
            }

            if (selectedJourney.availableSeats[0] < numPassengers) 
			{
                cout << "Not enough seats available. Please choose another train.\n";
            } 
			else 
			{
                selectedJourney.availableSeats[0] -= numPassengers;
                selectedJourney.totalPassengers += numPassengers;
                selectedJourney.totalRevenue += totalCost;

                cout << "Total Cost: $" << totalCost << endl;
                cout << "Tickets booked for Train " << choice << ".\n";
            }
        } 
		else 
		{
            cout << "Invalid choice. Please enter a number between 1 and 4 or 0 to exit.\n";
        }
    }

    int totalPassengersForDay = 0;
    int totalRevenueForDay = 0;
    TrainJourney mostPassengersJourney = trainSchedule[0];

    cout << "End of the Day Summary:\n";
    for (int i = 0; i < trainSchedule.size(); i++) 
	{
        TrainJourney& journey = trainSchedule[i];
        cout << "Train " << (i + 1) << ": " << journey.departureTime << " to " << journey.returnTime << "\n";
        cout << "Total Passengers: " << journey.totalPassengers << "\n";
        cout << "Total Revenue: $" << journey.totalRevenue << "\n";
        
        totalPassengersForDay += journey.totalPassengers;
        totalRevenueForDay += journey.totalRevenue;

        if (journey.totalPassengers > mostPassengersJourney.totalPassengers) 
		{
            mostPassengersJourney = journey;
        }
    }

    cout << "Total Passengers for the Day: " << totalPassengersForDay << "\n";
    cout << "Total Revenue for the Day: $" << totalRevenueForDay << "\n";
    cout << "Train Journey with the Most Passengers: Train " << (mostPassengersJourney.departureTime) << " to " << mostPassengersJourney.returnTime << "\n";

    return 0;
}
