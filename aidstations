import dateutil.parser
import utm

class AidStation (object):
  def __init__(self, name, lat, lon, radius):
    self.lat, self.lon = lat, lon
    self.name = name
    self.radius = radius
    self.u = utm.from_latlon(lat, lon)

  def is_close (self, n, e):
    return (self.u[0]-n)**2 + (self.u[1]-e)**2 < self.radius**2
    

aid_stations = { AidStation("Raven", 30.614473677260094, -95.5330045674925, 50),
                 AidStation("Gate/Damnation", 30.614900434413507, -95.52029179036619, 50),
                 AidStation("Nature Center", 30.626494736979005, -95.5279764533043, 50),
               }

def get_closest_station (n, e):
  closest = None
  for station in aid_stations:
    if station.is_close(n, e):
        closest = station
        break
  return closest

def racetime (start, t):
  # return datetime.datetime.fromisoformat(t) - datetime.datetime.fromisoformat(start)
  return dateutil.parser.isoparse(t) - dateutil.parser.isoparse(start)

with open("track.gpx") as f:
  lines = [l.strip() for l in f.read().split('\n')]

data = []
start_time = ""
timestamp = ""
lat, lon = "", ""
for l in lines[4:]:
  if l.startswith("<trkpt"):
      vals = l.split("\"")
      lat, lon = vals[1], vals[3]
      u = utm.from_latlon(float(lat), float(lon))

  if l.startswith("<time>"):
      timestamp = l[6:][:l[6:].index("<")]
      if start_time == "":
        start_time = timestamp
      else:
        closest = get_closest_station(u[0], u[1])
        station = closest.name if closest else ""
        data.append("{} {} {} {}".format(racetime(start_time, timestamp), u[0], u[1], station))

with open("points.txt", "w") as f:
  f.write("\n".join(data))


  
