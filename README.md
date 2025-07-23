# python--task-1
import requests

# Convert Celsius to Fahrenheit and Kelvin
def convert_temperature(celsius):
    fahrenheit = (celsius * 9/5) + 32
    kelvin = celsius + 273.15
    return round(celsius, 2), round(fahrenheit, 2), round(kelvin, 2)

# Fetch temperature from OpenWeatherMap API
def get_temperature(city, api_key):
    url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": f"{city},IN",
        "appid": api_key,
        "units": "metric"
    }

    response = requests.get(url, params=params)
    data = response.json()

    if response.status_code == 200:
        temp_celsius = data["main"]["temp"]
        return convert_temperature(temp_celsius)
    else:
        raise Exception(data.get("message", "Failed to get weather data."))

# Main program
def main():
    print("🌡️ Indian City Temperature Checker 🌡️")
    cities = ["Delhi", "Mumbai", "Chennai", "Kolkata", "Bengaluru", "Hyderabad", "Ahmedabad", "Pune", "Jaipur", "Lucknow"]

    # Show numbered list of cities
    for idx, city in enumerate(cities, 1):
        print(f"{idx}. {city}")

    try:
        choice = int(input("\nSelect a city by number (1-10): "))
        if 1 <= choice <= len(cities):
            selected_city = cities[choice - 1]
        else:
            print("❌ Invalid choice. Please choose a number between 1 and 10.")
            return
    except ValueError:
        print("❌ Please enter a valid number.")
        return

    api_key = input("🔑 Enter your OpenWeatherMap API key: ")

    try:
        celsius, fahrenheit, kelvin = get_temperature(selected_city, api_key)
        print(f"\n📍 City: {selected_city}")
        print(f"🌡️ Temperature in Celsius:    {celsius} °C")
        print(f"🌡️ Temperature in Fahrenheit: {fahrenheit} °F")
        print(f"🌡️ Temperature in Kelvin:     {kelvin} K")
    except Exception as e:
        print(f"\n❌ Error: {e}")

if __name__ == "__main__":
    main()
 
