import tkinter as tk
from tkinter import messagebox
import requests

API_KEY = "46ca95f4aef917f6c90174a13998a4a4"  # Replace with your OpenWeatherMap API key

def get_weather():
    city = city_entry.get()
    if not city:
        messagebox.showwarning("Input Error", "Please enter a city name.")
        return
    
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}&units=metric"
    
    try:
        response = requests.get(url)
        data = response.json()
        
        if data.get("cod") != 200:
            messagebox.showerror("Error", f"City not found: {city}")
            return

        temp = data["main"]["temp"]
        desc = data["weather"][0]["description"]
        humidity = data["main"]["humidity"]
        wind = data["wind"]["speed"]
        
        result_label.config(
            text=f"🌡 Temperature: {temp}°C\n"
                 f"🌥 Weather: {desc.title()}\n"
                 f"💧 Humidity: {humidity}%\n"
                 f"🌬 Wind Speed: {wind} m/s"
        )
    except Exception as e:
        messagebox.showerror("Error", str(e))

# GUI
root = tk.Tk()
root.title("Weather App")
root.geometry("350x300")
root.configure(bg="#dff0f8")

tk.Label(root, text="Enter City Name", font=("Arial", 14), bg="#dff0f8").pack(pady=10)

city_entry = tk.Entry(root, font=("Arial", 14), justify='center')
city_entry.pack(pady=5)

tk.Button(root, text="Get Weather", font=("Arial", 12), command=get_weather).pack(pady=10)

result_label = tk.Label(root, text="", font=("Arial", 12), bg="#dff0f8", justify="left")
result_label.pack(pady=20)

root.mainloop()
