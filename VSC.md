Codigo del VSC

    from flask import Flask, jsonify
    import psycopg2

    app = Flask(__name__)
    app.json.sort_keys = False

    def obtener_conexion():
    return psycopg2.connect(
        host="localhost",
        database="Tablasdelasemana",
        user="postgres",
        password="12345",
        port="5432"
    )

# ------------------------------------------
    @app.route("/")
    def inicio():
    conexion = obtener_conexion()
    cursor = conexion.cursor()

    # Lunes
    cursor.execute("SELECT * FROM lunes")
    lunes = cursor.fetchall()
    lista_lunes = []
    for act in lunes:
    lista_lunes.append({"id": act[0],            "actividad": act[1]})    
        
    # Martes
    cursor.execute("SELECT * FROM martes")
    martes = cursor.fetchall()
    lista_martes = []
    for act in martes:
    lista_martes.append({"id": act[0],           "actividad": act[1]})    
        
    # Miércoles
    cursor.execute("SELECT * FROM miercoles")
    miercoles = cursor.fetchall()
    lista_miercoles = []
    for act in miercoles:
    lista_miercoles.append({"id": act[0],        "actividad": act[1]})    
        
    # Jueves
    cursor.execute("SELECT * FROM jueves")
    jueves = cursor.fetchall()
    lista_jueves = []
    for act in jueves:
    lista_jueves.append({"id": act[0],           "actividad": act[1], "observacion":          act[2]})    
        
    cursor.close()
    conexion.close()

    # Itinerario siguiendo el orden
    itinerario_ordenado = {
        "lunes": lista_lunes,
        "martes": lista_martes,
        "miercoles": lista_miercoles,
        "jueves": lista_jueves
    }
    return jsonify(itinerario_ordenado)

# ------------------------------------

    @app.route("/lunes")
    def ruta_lunes():
    conexion = obtener_conexion()
    cursor = conexion.cursor()
    cursor.execute("SELECT * FROM lunes")
    datos = cursor.fetchall()
    
    lista = []
    for act in datos:
    lista.append({"id": act[0], "actividad":     act[1]})
        
    cursor.close()
    conexion.close()
    return jsonify({"lunes": lista})

    @app.route("/martes")
    def ruta_martes():
    conexion = obtener_conexion()
    cursor = conexion.cursor()
    cursor.execute("SELECT * FROM martes")
    datos = cursor.fetchall()
    
    lista = []
    for act in datos:
    lista.append({"id": act[0], "actividad":     act[1]})
        
    cursor.close()
    conexion.close()
    return jsonify({"martes": lista})

    @app.route("/miercoles")
    def ruta_miercoles():
    conexion = obtener_conexion()
    cursor = conexion.cursor()
    cursor.execute("SELECT * FROM miercoles")
    datos = cursor.fetchall()
    
    lista = []
    for act in datos:
    lista.append({"id": act[0],                  "actividad": act[1]})
        
    cursor.close()
    conexion.close()
    return jsonify({"miercoles": lista})

    @app.route("/jueves")
    def ruta_jueves():
    conexion = obtener_conexion()
    cursor = conexion.cursor()
    cursor.execute("SELECT * FROM jueves")
    datos = cursor.fetchall()
    
    lista = []
    for act in datos:
    lista.append({"id": act[0], "actividad":     act[1], "observacion": act[2]})
        
    cursor.close()
    conexion.close()
    return jsonify({"jueves": lista})

    if __name__ == "__main__":
    app.run(debug=True)
