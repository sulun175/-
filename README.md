# -
python天气查询CLI工具
import requests
import json

def get_weather(api_key, city):
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    params = {
        'q': city,
        'appid': api_key,
        'units': 'metric'  # 使用摄氏温度
    }
    
    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()  # 检查请求是否成功
        weather_data = response.json()
        
        # 解析返回的JSON数据
        if weather_data['cod'] != 404:
            main_info = weather_data['main']
            temperature = main_info['temp']
            humidity = main_info['humidity']
            pressure = main_info['pressure']
            weather_desc = weather_data['weather'][0]['description']
            
            print(f"\n天气信息 - {city}:")
            print(f"温度: {temperature}°C")
            print(f"湿度: {humidity}%")
            print(f"气压: {pressure} hPa")
            print(f"天气状况: {weather_desc.capitalize()}")
        else:
            print("城市未找到，请检查城市名称是否正确。")
            
    except requests.exceptions.RequestException as e:
        print(f"获取天气信息时出错: {e}")

def main():
    print("=== 天气查询系统 ===")
    
    
    API_KEY = "your_api_key_here"
    
    while True:
        print("\n1. 查询天气")
        print("2. 退出系统")
        choice = input("请选择操作 (1/2): ")
        
        if choice == '1':
            city = input("请输入城市名称: ")
            get_weather(API_KEY, city)
        elif choice == '2':
            print("感谢使用天气查询系统，再见！")
            break
        else:
            print("无效的选择，请重新输入。")

if __name__ == "__main__":
    main()
