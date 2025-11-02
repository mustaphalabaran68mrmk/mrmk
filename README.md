
mktrack/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   ├── crud.py
│   │   ├── deps.py
│   │   ├── auth.py
│   │   ├── routers/
│   │   │   ├── auth.py
│   │   │   ├── devices.py
│   │   │   └── locations.py
│   │   └── core/
│   │       ├── config.py
│   │       └── security.py
│   ├── Dockerfile
│   └── requirements.txt
├── dashboard/
│   ├── package.json
│   ├── src/
│   │   ├── App.jsx
│   │   ├── pages/Dashboard.jsx
│   │   ├── pages/Login.jsx
│   │   └── components/MapView.jsx
│   └── tailwind.config.js
├── mobile/
│   ├── package.json
│   ├── App.js
│   ├── src/
│   │   ├── screens/LoginScreen.js
│   │   ├── screens/TrackingScreen.js
│   │   └── services/api.js
│   └── android/ ios/ (if bare)
├── docker-compose.yml
└── README.md
Write to Sarkin Mota
from fastapi import FastAPI
from .routers import auth, devices, locations

app = FastAPI(title="MKTrack API")
app.include_router(auth.router, prefix="/auth")
app.include_router(devices.router, prefix="/devices")
app.include_router(locations.router, prefix="/locations")

@app.get('/')
async def root():
    return {"msg": "MKTrack backend running"}
Sarkin
import React, {useEffect, useState} from 'react'
import { Button, View, Text } from 'react-native'
import * as Location from 'expo-location'
import axios from 'axios'

export default function App(){
  const [hasPermission, setHasPermission] = useState(false)

  useEffect(()=>{ (async()=>{
    const {status} = await Location.requestForegroundPermissionsAsync()
    setHasPermission(status === 'granted')
  })() }, [])

  const sendLocation = async ()=>{
    const loc = await Location.getCurrentPositionAsync({accuracy:Location.Accuracy.Balanced})
    await axios.post('https://your-backend.com/locations',{
      imei: 'DEVICE_IMEI_PLACEHOLDER',
      latitude: loc.coords.latitude,
      longitude: loc.coords.longitude
    })
  }

  if(!hasPermission) return <View><Text>Need location permission</Text></View>
  return (<View><Button title="Send Location" onPress={sendLocation} /></View>)
}
Sarkin
# backend
DATABASE_URL=postgresql://user:pass@db:5432/mktrack
SECRET_KEY=supersecretjwt
RETENTION_DAYS=30
REDIS_URL=redis://redis:6379

# dashboard
REACT_APP_API_URL=https://api.mktrack.com
REACT_APP_MAP_KEY=YOUR_GOOGLE_MAPS_KEY

# mobile
EXPO_PUBLIC_API_URL=https://api.mktrack.com
Sarkin
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    env_file:
      - .env.backend
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: mktrack
      POSTGRES_PASSWORD: example
      POSTGRES_DB: mktrack
    volumes:
      - db-data:/var/lib/postgresql/data
  redis:
    image: redis:7
volumes:
  db-data:
Sarkin
MKTrack requires your explicit consent to collect device IMEI and location data. This data is used only for fleet management and device recovery. You may disable tracking at any time. Location data is retained for 30 days and then deleted. By tapping "I Agree" you consent to this collection and processing.
Write to Sarkin Mota

  
