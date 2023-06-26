# PrimerRepo
import tkinter as tk
from tkinter import messagebox
import numpy as np
import matplotlib.pyplot as plt

class Figura:
    def calcular_perimetro(self):
        pass

    def calcular_area(self):
        pass

    def calcular_superficie_esfera(self):
        pass

    def calcular_volumen_esfera(self):
        pass

    def calcular_volumen_cono(self):
        pass


class Circulo(Figura):
    def _init_(self, radio):
        self.radio = radio

    def calcular_perimetro(self):
        return 2 * np.pi * self.radio

    def calcular_area(self):
        return np.pi * self.radio ** 2

    def calcular_superficie_esfera(self):
        return 4 * np.pi * self.radio ** 2

    def calcular_volumen_esfera(self):
        return (4/3) * np.pi * self.radio ** 3


class Triangulo(Figura):
    def _init_(self, base, altura, radio=None):
        self.base = base
        self.altura = altura
        self.radio = radio

    def calcular_perimetro(self):
        return self.base + 2 * np.sqrt((self.base/2) * 2 + self.altura * 2)

    def calcular_area(self):
        return 0.5 * self.base * self.altura

    def calcular_superficie_triangulo(self):
        return self.base * self.altura

    def calcular_volumen_cono(self):
        if self.radio:
            return (1/3) * np.pi * self.radio ** 2 * self.altura
        else:
            raise ValueError("El radio no ha sido especificado")


class Rectangulo(Figura):
    def _init_(self, base, altura, radio=None):
        self.base = base
        self.altura = altura
        self.radio = radio

    def calcular_perimetro(self):
        return 2 * (self.base + self.altura)

    def calcular_area(self):
        return self.base * self.altura

    def calcular_superficie_cilindro(self):
        if self.radio:
            return 2 * np.pi * self.radio ** 2 + self.calcular_perimetro() * self.altura
        else:
            raise ValueError("El radio no ha sido especificado")

    def calcular_volumen_cilindro(self):
        if self.radio:
            return np.pi * self.radio ** 2 * self.altura
        else:
            raise ValueError("El radio no ha sido especificado")


class FiguraApp:
    def _init_(self, root):
        self.root = root
        self.root.title("Calculadora de Figuras")
        self.root.geometry("300x250")

        self.label_figuras = tk.Label(self.root, text="Seleccione una figura:")
        self.label_figuras.pack()

        self.frame_figuras = tk.Frame(self.root)
        self.frame_figuras.pack()

        self.var_figura = tk.StringVar()
        self.var_figura.set("Círculo")

        self.radiobutton_circulo = tk.Radiobutton(self.frame_figuras, text="Círculo", variable=self.var_figura, value="Círculo", command=self.mostrar_datos)
        self.radiobutton_circulo.grid(row=0, column=0, padx=10, pady=10)

        self.radiobutton_triangulo = tk.Radiobutton(self.frame_figuras, text="Triángulo", variable=self.var_figura, value="Triángulo", command=self.mostrar_datos)
        self.radiobutton_triangulo.grid(row=0, column=1, padx=10, pady=10)

        self.radiobutton_rectangulo = tk.Radiobutton(self.frame_figuras, text="Rectángulo", variable=self.var_figura, value="Rectángulo", command=self.mostrar_datos)
        self.radiobutton_rectangulo.grid(row=0, column=2, padx=10, pady=10)

        self.label_base = tk.Label(self.root, text="Base:")
        self.label_base.pack()
        self.entry_base = tk.Entry(self.root)
        self.entry_base.pack()

        self.label_altura = tk.Label(self.root, text="Altura:")
        self.label_altura.pack()
        self.entry_altura = tk.Entry(self.root)
        self.entry_altura.pack()

        self.label_largo = tk.Label(self.root, text="Largo:")
        self.label_largo.pack()
        self.entry_largo = tk.Entry(self.root)
        self.entry_largo.pack()

        self.label_radio = tk.Label(self.root, text="Radio (opcional):")
        self.label_radio.pack()
        self.entry_radio = tk.Entry(self.root)
        self.entry_radio.pack()

        self.button_calcular = tk.Button(self.root, text="Calcular", command=self.calcular)
        self.button_calcular.pack()

    def mostrar_datos(self):
        figura = self.var_figura.get()

        if figura == "Círculo":
            self.label_base.config(text="Radio:")
            self.label_altura.pack_forget()
            self.entry_altura.pack_forget()
            self.label_largo.pack_forget()
            self.entry_largo.pack_forget()
        elif figura == "Triángulo":
            self.label_base.config(text="Base:")
            self.label_altura.config(text="Altura:")
            self.label_altura.pack()
            self.entry_altura.pack()
            self.label_largo.pack_forget()
            self.entry_largo.pack_forget()
        elif figura == "Rectángulo":
            self.label_base.config(text="Base:")
            self.label_altura.config(text="Altura:")
            self.label_altura.pack()
            self.entry_altura.pack()
            self.label_largo.pack()
            self.entry_largo.pack()

    def calcular(self):
        figura = self.var_figura.get()

        if figura == "Círculo":
            radio = float(self.entry_base.get())
            figura = Circulo(radio)

            resultados = {
                "Perímetro": figura.calcular_perimetro(),
                "Área": figura.calcular_area(),
                "Superficie Esfera": figura.calcular_superficie_esfera(),
                "Volumen Esfera": figura.calcular_volumen_esfera()
            }
        elif figura == "Triángulo":
            base = float(self.entry_base.get())
            altura = float(self.entry_altura.get())
            radio = float(self.entry_radio.get()) if self.entry_radio.get() else None
            figura = Triangulo(base, altura, radio)

            resultados = {
                "Perímetro": figura.calcular_perimetro(),
                "Área": figura.calcular_area(),
                "Superficie Triángulo": figura.calcular_superficie_triangulo()
            }

            if radio:
                resultados["Volumen Cono"] = figura.calcular_volumen_cono()
        elif figura == "Rectángulo":
            base = float(self.entry_base.get())
            altura = float(self.entry_altura.get())
            radio = float(self.entry_radio.get()) if self.entry_radio.get() else None
            figura = Rectangulo(base, altura, radio)

            resultados = {
                "Perímetro": figura.calcular_perimetro(),
                "Área": figura.calcular_area()
            }

            if radio:
                resultados["Superficie Cilindro"] = figura.calcular_superficie_cilindro()
                resultados["Volumen Cilindro"] = figura.calcular_volumen_cilindro()

        messagebox.showinfo("Resultados", "\n".join([f"{k}: {v}" for k, v in resultados.items()]))

        if figura._class.name_ == "Triangulo":
            plt.plot([0, figura.base, figura.base/2, 0], [0, 0, figura.altura, 0])
            plt.fill([0, figura.base, figura.base/2, 0], [0, 0, figura.altura, 0])
        elif figura._class.name_ == "Rectangulo":
            plt.plot([0, figura.base, figura.base, 0, 0], [0, 0, figura.altura, figura.altura, 0])
            plt.fill([0, figura.base, figura.base, 0, 0], [0, 0, figura.altura, figura.altura, 0])
        else:
            circle = plt.Circle((0, 0), figura.radio, fill=False)
            plt.gca().add_patch(circle)

        plt.axis('equal')
        plt.show()

root = tk.Tk()
app = FiguraApp(root)
root.mainloop()
