# ***Calculadora de Moralidad***
Un día me hice una pregunta, como sé si no estoy equivocado a la hora de juzgar una situación o dilema moral, para responder a ello cree la calculadora en porcentajes que tan correcta es una situación o decisión en base a marcos filosóficos.
```python
"""
╔══════════════════════════════════════════════════════════╗
║          ANALIZADOR DE DECISIONES MORALES                ║
║   Basado en los principales marcos éticos filosóficos    ║
╚══════════════════════════════════════════════════════════╝
"""

# ─── Marcos éticos disponibles ─────────────────────────────────────────────────

MARCOS_ETICOS = {
    "utilitarismo": {
        "nombre":      "⚖️  Utilitarismo",
        "filosofo":    "Jeremy Bentham / John Stuart Mill",
        "descripcion": "Busca la mayor felicidad para el mayor número de personas.",
        "preguntas": [
            ("¿Cuántas personas se beneficiarían de esta acción?",
             ["Muchas (>10)",  "Algunas (3-10)", "Pocas (1-2)", "Ninguna"]),
            ("¿Cuántas personas se verían perjudicadas?",
             ["Ninguna",       "Pocas (1-2)",    "Algunas (3-10)", "Muchas (>10)"]),
            ("¿El beneficio es a largo o corto plazo?",
             ["Largo plazo",   "Ambos",          "Corto plazo",    "Ninguno"]),
        ]
    },
    "deontologia": {
        "nombre":      "📜 Deontología (Kant)",
        "filosofo":    "Immanuel Kant",
        "descripcion": "Las acciones son morales si respetan el deber y los derechos universales.",
        "preguntas": [
            ("¿Podrías desear que TODOS hicieran lo mismo en tu situación?",
             ["Sí, sin duda",  "Probablemente",  "Dudoso",         "No"]),
            ("¿Tratas a las personas como fines en sí mismas (no solo como medios)?",
             ["Sí, siempre",   "En su mayoría",  "Parcialmente",   "No"]),
            ("¿Respetas los derechos fundamentales de todos los involucrados?",
             ["Completamente", "En su mayoría",  "Parcialmente",   "No"]),
        ]
    },
    "virtud": {
        "nombre":      "🌿 Ética de la Virtud",
        "filosofo":    "Aristóteles",
        "descripcion": "Pregunta qué haría una persona virtuosa y de buen carácter.",
        "preguntas": [
            ("¿Esta acción refleja valentía, justicia u honestidad?",
             ["Mucho",         "Bastante",       "Algo",           "Nada"]),
            ("¿Te sentiría orgulloso/a de esta decisión ante alguien que admiras?",
             ["Totalmente",    "Probablemente",  "No estoy seguro","No"]),
            ("¿La acción refuerza hábitos que te harán mejor persona?",
             ["Sí, claramente","Posiblemente",   "Neutro",         "Me perjudica"]),
        ]
    },
    "contractualismo": {
        "nombre":      "🤝 Contractualismo",
        "filosofo":    "John Rawls",
        "descripcion": "¿Sería justa esta acción si no supieras qué rol tendrías en ella?",
        "preguntas": [
            ("Si no supieras si serías el afectado o el beneficiado, ¿lo aceptarías?",
             ["Sin duda",      "Probablemente",  "Con reservas",   "No"]),
            ("¿La acción sería aceptable para los miembros más vulnerables?",
             ["Sí",            "Mayormente sí",  "Dudoso",         "No"]),
            ("¿Podrías defender esta decisión públicamente ante todos los afectados?",
             ["Sí, fácilmente","Con dificultad", "Apenas",         "No podría"]),
        ]
    },
}

PUNTOS = [3, 2, 1, 0]   # puntos por cada opción (índice 0 = mejor)


# ─── Utilidades de consola ──────────────────────────────────────────────────────

def linea(char="─", n=60):
    print(char * n)

def titulo(texto, char="═"):
    linea(char)
    print(f"  {texto}")
    linea(char)

def pregunta_si_no(texto):
    while True:
        r = input(f"{texto} (s/n): ").strip().lower()
        if r in ("s", "si", "sí", "y", "yes"):
            return True
        if r in ("n", "no"):
            return False
        print("  ⚠️  Por favor responde 's' o 'n'.")

def elegir_opcion(opciones, etiqueta="Opción"):
    for i, op in enumerate(opciones, 1):
        print(f"    {i}. {op}")
    while True:
        try:
            r = int(input(f"  ▶  Elige {etiqueta} (1-{len(opciones)}): "))
            if 1 <= r <= len(opciones):
                return r - 1          # índice 0-based
            print(f"  ⚠️  Ingresa un número entre 1 y {len(opciones)}.")
        except ValueError:
            print("  ⚠️  Ingresa un número válido.")


# ─── Evaluación por marco ───────────────────────────────────────────────────────

def evaluar_marco(clave, marco):
    titulo(marco["nombre"])
    print(f"  🧠  Filósofo(s): {marco['filosofo']}")
    print(f"  💡  Principio  : {marco['descripcion']}")
    linea()
    total = 0
    for texto, opciones in marco["preguntas"]:
        print(f"\n  ❓  {texto}")
        idx   = elegir_opcion(opciones)
        total += PUNTOS[idx]
        print(f"       → {opciones[idx]}")
    maximo = len(marco["preguntas"]) * 3
    return total, maximo


def barra(puntos, maximo, largo=20):
    llenos   = round((puntos / maximo) * largo)
    vacios   = largo - llenos
    return "█" * llenos + "░" * vacios


def veredicto(porcentaje):
    if porcentaje >= 75:
        return "✅  MORALMENTE JUSTIFICADA"
    elif porcentaje >= 50:
        return "⚠️   MORALMENTE DUDOSA"
    elif porcentaje >= 25:
        return "🔶  MORALMENTE PROBLEMÁTICA"
    else:
        return "❌  MORALMENTE INJUSTIFICADA"


# ─── Flujo principal ─────────────────────────────────────────────────────────────

def main():
    print()
    titulo("ANALIZADOR DE DECISIONES MORALES", "═")
    print("""
  Este programa analiza tu dilema moral desde cuatro grandes
  tradiciones filosóficas y te ofrece una visión integral.

  ⚡  Responde con honestidad para obtener el mejor análisis.
""")

    # 1. Descripción del dilema
    linea()
    print("  📝  Describe brevemente tu dilema moral:")
    dilema = input("  > ").strip()
    if not dilema:
        dilema = "(dilema no especificado)"

    # 2. Elegir marcos a usar
    print("\n  ¿Qué marcos éticos deseas aplicar?")
    claves_disponibles = list(MARCOS_ETICOS.keys())
    seleccionados      = []

    for clave in claves_disponibles:
        nombre = MARCOS_ETICOS[clave]["nombre"]
        if pregunta_si_no(f"  ¿Incluir {nombre}?"):
            seleccionados.append(clave)

    if not seleccionados:
        print("\n  ⚠️  No seleccionaste ningún marco. Se usarán todos por defecto.\n")
        seleccionados = claves_disponibles

    # 3. Evaluación
    resultados = {}
    for clave in seleccionados:
        print()
        puntos, maximo          = evaluar_marco(clave, MARCOS_ETICOS[clave])
        resultados[clave]       = (puntos, maximo)

    # 4. Informe final
    print("\n")
    titulo("📊 INFORME FINAL", "═")
    print(f"  Dilema evaluado: «{dilema}»\n")

    total_p = total_m = 0
    for clave, (pts, mx) in resultados.items():
        pct  = (pts / mx) * 100
        bar  = barra(pts, mx)
        vrd  = veredicto(pct)
        nombre = MARCOS_ETICOS[clave]["nombre"]
        print(f"  {nombre}")
        print(f"    [{bar}] {pct:5.1f}%  →  {vrd}")
        print()
        total_p += pts
        total_m += mx

    # Veredicto global
    pct_global = (total_p / total_m) * 100
    linea("─")
    print(f"  VEREDICTO GLOBAL  [{barra(total_p, total_m)}] {pct_global:.1f}%")
    print(f"  {veredicto(pct_global)}")
    linea("─")

    # 5. Recomendación cualitativa
    print("""
  📌  Disclaimer:
  ─────────────────────────────────────────────────────────
  • Ningún análisis automático reemplaza tu juicio personal.
  • Los marcos éticos son herramientas, no verdades absolutas.
  • Consulta a personas de confianza ante decisiones críticas.
  • La reflexión constante es el verdadero motor moral.
    """)

    # 6. ¿Repetir?
    linea()
    if pregunta_si_no("  ¿Deseas analizar otro dilema?"):
        main()
    else:
        print("\n  ✨  ¡Buena suerte con tu decisión!\n")


# ─── Entrada ────────────────────────────────────────────────────────────────────

if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n\n  Programa interrumpido. ¡Hasta pronto!\n")
``` 
