{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Cálculo de la probabilidad de al menos 1 victoria tras n tiradas en un juego con una probabilidad p de ganar en una tirada"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$\\Large P = {\\sum_{m=1}^n} {\\begin{equation} {n \\choose m} {\\frac {p^{m} (1-p)^{n-m}}{100^{n}}} \\end{equation}}$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Eso es lo que sale tras darle unas vueltas e inducir la fórmula, siendo:  \n",
    "\n",
    "p: Probabilidad de ganar en 1 tirada.  \n",
    "n: Número de tiradas  \n",
    "P: Probabilidad de al menos 1 victoria tras n tiradas\n",
    "\n",
    "Por ejemplo, ¿qué probabilidad hay de que ocurra un suceso que tiene un 1% de probabilidades de ocurrir tras 100 intentos o tiradas? Que ocurra en 1 de cada 100 no significa que en un grupo de 100 haya 1 con seguridad. Esta P por tanto no es 100%, sino, como veremos, cerca del 63%  \n",
    "\n",
    "¿Cuál es la probabilidad de que ocurra al menos 1 suceso cuando p es 1 entre 1000, tras 1000 tiradas? ¿Y cuando p es 1 entre 10.000 tras 10.000 tiradas? Etc. ¿A qué tiende P cuando p es muy pequeño y n es muy grande proporcionalmente hablando? Veremos que **P tiende a $1 - \\Large \\frac{1}{e}$** cuando p=1/chances muy pequeña y n=chances muy grande."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Explicación de la fórmula: desarrollo para p = 1% y n = 4:**"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$P = \\frac{1}{100}·\\frac{99}{100} ·\\frac{99}{100}·\\frac{99}{100} · 4  +  \\frac{1}{100}·\\frac{1}{100} ·\\frac{99}{100}·\\frac{99}{100} ·6 +  \\frac{1}{100}·\\frac{1}{100} ·\\frac{1}{100}·\\frac{99}{100}·4  + \\frac{1}{100}·\\frac{1}{100} ·\\frac{1}{100}·\\frac{1}{100} ·1 $  \n",
    "\n",
    "Probabilidad de 1 acierto (el 1º y el resto no, el 2º y el resto no, etc., 4 en total) + probabilidad de 2 aciertos (1º y 2º, 1º y 3º, etc., en total 6 combinaciones) + probabilidad de 3 aciertos + probabilidad de 4 aciertos. Por tanto, P respresenta la **probabilidad de acertar al menos 1 vez**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "720"
      ]
     },
     "execution_count": 1,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def factorial(x):\n",
    "    resultado = 1\n",
    "    i = 1\n",
    "    while i <= x:\n",
    "        resultado = resultado*i\n",
    "        i += 1\n",
    "    return resultado\n",
    "factorial(6)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "120"
      ]
     },
     "execution_count": 36,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def combinatorio(n,m):\n",
    "    \"\"\"Devuelve el número conbinatorio n sobre m\"\"\"\n",
    "    resultado = factorial(n)/(factorial(m)*factorial(n-m))\n",
    "    return int(resultado)\n",
    "combinatorio(10,3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [],
   "source": [
    "def P(p, tiradas):\n",
    "    \"\"\"Esta función devuelve la probabilidad de obtener al menos 1 victoria al jugar 'tiradas' \n",
    "    veces a un juego con una probabilidad p de ganar en cada tirada\"\"\"\n",
    "    \"\"\"p: Probabilidad en tanto por ciento (%)\"\"\"\n",
    "    \n",
    "    prob = 0\n",
    "    for m in range(1,tiradas+1):\n",
    "        p_m = combinatorio(tiradas,m)*(p**m * (100-p)**(tiradas - m))/(100**tiradas)\n",
    "        prob += p_m\n",
    "    \n",
    "    return prob"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.029700999999999998"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P(1,3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 171,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "120"
      ]
     },
     "execution_count": 171,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def combinatorio_actualizado(n,m):\n",
    "    \"\"\"Devuelve el número conbinatorio n sobre m\"\"\"\n",
    "    if m < n/2:\n",
    "        resultado = 1\n",
    "        k = 0\n",
    "        while k < m:\n",
    "            resultado *= (n-k) \n",
    "            k += 1\n",
    "        resultado /= factorial(m)\n",
    "    else:\n",
    "        resultado = 1\n",
    "        k = 0\n",
    "        while k < n-m-1:\n",
    "            resultado *= (n-k) \n",
    "            k += 1\n",
    "        resultado /= factorial(n-m)\n",
    "        \n",
    "    return int(resultado)\n",
    "\n",
    "combinatorio_actualizado(10,3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 195,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "120"
      ]
     },
     "execution_count": 195,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def combinatorio_actualizado(n,m):\n",
    "    \"\"\"Devuelve el número conbinatorio n sobre m\"\"\"\n",
    "    \"\"\"Otra manera de calcular el combinatorio simplificando para no tener que manejar numeros tan grandes\"\"\"\n",
    "    \n",
    "    resultado = 1\n",
    "    for k in range(m):\n",
    "        resultado *= (n-k)/(m-k)\n",
    "    return int(resultado)\n",
    "combinatorio_actualizado(10,3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 196,
   "metadata": {},
   "outputs": [],
   "source": [
    "def P_actualizado(p, tiradas):\n",
    "    \"\"\"Esta función devuelve la probabilidad de obtener al menos 1 victoria al jugar 'tiradas' \n",
    "    veces a un juego con una probabilidad p de ganar en cada tirada\"\"\"\n",
    "    \"\"\"p: Probabilidad en tanto por ciento (%)\"\"\"\n",
    "    \n",
    "    prob = 0\n",
    "    for m in range(1,tiradas+1):\n",
    "        p_m = combinatorio_actualizado(tiradas,m)*(p/(100-p))**m * ((100-p)/100)**tiradas\n",
    "        prob += p_m\n",
    "    \n",
    "    return prob"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "def P_actualizado_actualizado(p, tiradas):\n",
    "    \"\"\"Esta función devuelve la probabilidad de obtener al menos 1 victoria al jugar 'tiradas' \n",
    "    veces a un juego con una probabilidad p de ganar en cada tirada\"\"\"\n",
    "    \"\"\"p: Probabilidad en tanto por ciento (%)\"\"\"\n",
    "    \n",
    "    \"\"\"Para que valga para grandes valores de 'tiradas', sustituyo la función combinatorio\n",
    "    par que no tenga que calcular un número tan grande, intercalando los factores del combinatorio\n",
    "    con factores (p/100-p), que son pequeños\"\"\"\n",
    "    \n",
    "    prob = 0\n",
    "    for m in range(1,tiradas+1):\n",
    "        p_m = 1\n",
    "        for k in range (m): #Esto sustituye a combinatorio_actualizado\n",
    "            p_m *= ((tiradas-k)/(m-k))*(p/(100-p))\n",
    "        p_m *= ((100-p)/100)**tiradas\n",
    "        prob += p_m\n",
    "        \n",
    "        if m % 10000 == 0:\n",
    "            print(\"{} de {} completado\".format(m,tiradas))\n",
    "    \n",
    "    return prob"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.8646647161537687"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P_actualizado_actualizado(63.2120558, 2)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**P_actualizado_actualizado:**\n",
    "\n",
    "$\\Large {n \\choose m} {\\frac {p^{m} (1-p)^{n-m}}{100^{n}}} = {n \\choose m} {(\\frac{p}{100-p})^m} {(\\frac{100-p}{100})^n} = [\\frac{n}{m} · (\\frac{p}{100-p}) · \\frac{n-1}{m-1} · (\\frac{p}{100-p})]·{(\\frac{100-p}{100})^n}$  \n",
    "\n",
    "Esto es lo que he hecho en P_actualizado_actualizado: Intercalo factores grandes con factores pequeños en lugar de tener que calcular números combinatorios tan grandes cuando el resultado final no será tan grande. Con el combinatorio por separado, me daba error ya para el cálculo P_actualizado(1e-2,10000)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 188,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.6323045752291058"
      ]
     },
     "execution_count": 188,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P_actualizado(0.1,1000)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 204,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.6321389535664379"
      ]
     },
     "execution_count": 204,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P_actualizado_actualizado(1e-2,10000)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 211,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "10000 de 100000 completado\n",
      "20000 de 100000 completado\n",
      "30000 de 100000 completado\n",
      "40000 de 100000 completado\n",
      "50000 de 100000 completado\n",
      "60000 de 100000 completado\n",
      "70000 de 100000 completado\n",
      "80000 de 100000 completado\n",
      "90000 de 100000 completado\n",
      "100000 de 100000 completado\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "0.6321223982292868"
      ]
     },
     "execution_count": 211,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P_actualizado_actualizado(1e-3,100000)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 207,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.6321205588285577"
      ]
     },
     "execution_count": 207,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import math\n",
    "1-1/math.exp(1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 201,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.6323045752291058"
      ]
     },
     "execution_count": 201,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P_actualizado_actualizado(0.1,1000)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0\n",
      "0.01\n",
      "0.0199\n",
      "0.029700999999999998\n",
      "0.03940399\n",
      "0.0490099501\n",
      "0.058519850599\n",
      "0.06793465209301\n",
      "0.0772553055720799\n",
      "0.0864827525163591\n"
     ]
    }
   ],
   "source": [
    "for tiradas in range(10):\n",
    "    print(P(1,tiradas))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [],
   "source": [
    "import matplotlib.pyplot as plt"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[[0.5952680273216757],\n",
       " [0.5993153470484592],\n",
       " [0.6033221935779745],\n",
       " [0.6072889716421948],\n",
       " [0.6112160819257727],\n",
       " [0.6151039211065151],\n",
       " [0.6189528818954501],\n",
       " [0.6227633530764957],\n",
       " [0.6265357195457306],\n",
       " [0.6302703623502731]]"
      ]
     },
     "execution_count": 39,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "lista = []\n",
    "for tiradas in range(100):\n",
    "    lista.append([P(1,tiradas)])\n",
    "lista[-10:]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 129,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0/1000\n",
      "50/1000\n",
      "100/1000\n",
      "150/1000\n",
      "200/1000\n",
      "250/1000\n",
      "300/1000\n",
      "350/1000\n",
      "400/1000\n",
      "450/1000\n",
      "500/1000\n",
      "550/1000\n",
      "600/1000\n",
      "650/1000\n",
      "700/1000\n",
      "750/1000\n",
      "800/1000\n",
      "850/1000\n",
      "900/1000\n",
      "950/1000\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "[[0.9999546038915809],\n",
       " [0.9999550578526655],\n",
       " [0.9999555072741386],\n",
       " [0.999955952201397],\n",
       " [0.9999563926793827]]"
      ]
     },
     "execution_count": 129,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "lista = []\n",
    "for tiradas in range(1000):\n",
    "    lista.append([P(1,tiradas)])\n",
    "    if tiradas % 50 == 0:\n",
    "        print(\"{}/1000\".format(tiradas))\n",
    "lista[-5:]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 130,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[<matplotlib.lines.Line2D at 0x1ef05282ec8>]"
      ]
     },
     "execution_count": 130,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.plot(lista)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 138,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "p = 1\n",
      "1 tiradas: 1.0% probabilidad de al menos 1 victoria\n",
      "5 tiradas: 4.90099501% probabilidad de al menos 1 victoria\n",
      "10 tiradas: 9.561792499119552% probabilidad de al menos 1 victoria\n",
      "25 tiradas: 22.217864060085322% probabilidad de al menos 1 victoria\n",
      "100 tiradas: 63.396765872677044% probabilidad de al menos 1 victoria\n",
      "200 tiradas: 86.60203251420381% probabilidad de al menos 1 victoria\n",
      "500 tiradas: 99.34295169575856% probabilidad de al menos 1 victoria\n",
      "750 tiradas: 99.9467406394113% probabilidad de al menos 1 victoria\n"
     ]
    }
   ],
   "source": [
    "print(\"p = 1\")\n",
    "for tiradas in [1,5,10,25,100,200,500,750]:\n",
    "    pp = lista[tiradas][0]\n",
    "    p = round(lista[tiradas][0],3)\n",
    "    print(\"{} tiradas: {}% probabilidad de al menos 1 victoria\".format(tiradas,pp*100))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 127,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "9.561792499119552"
      ]
     },
     "execution_count": 127,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P(1,10)*100"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Esto era para p = 1, **veamos más valores de p**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 84,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.9276581118159495"
      ]
     },
     "execution_count": 84,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "P(2,130)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 85,
   "metadata": {},
   "outputs": [],
   "source": [
    "numeros = [1,2,3]\n",
    "numeros.append(P(2,130))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 86,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[1, 2, 3, 0.9276581118159495]"
      ]
     },
     "execution_count": 86,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "numeros"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 79,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "p = 2 completado\n",
      "p = 10 completado\n",
      "p = 25 completado\n",
      "p = 50 completado\n",
      "p = 75 completado\n"
     ]
    }
   ],
   "source": [
    "Lista = []\n",
    "for p in [2,10,25,50,75]:\n",
    "    lista = []\n",
    "    for tiradas in range(500):\n",
    "        lista.append(P(p,tiradas))\n",
    "    print(\"p = {} completado\".format(p))\n",
    "    Lista.append(lista)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 139,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "\n",
      "p = 2\n",
      "1 tiradas: 2.0% probabilidad de al menos 1 victoria\n",
      "2 tiradas: 3.9599999999999995% probabilidad de al menos 1 victoria\n",
      "4 tiradas: 7.763183999999999% probabilidad de al menos 1 victoria\n",
      "10 tiradas: 18.29271931124531% probabilidad de al menos 1 victoria\n",
      "25 tiradas: 39.65352702211031% probabilidad de al menos 1 victoria\n",
      "50 tiradas: 63.58303199128829% probabilidad de al menos 1 victoria\n",
      "100 tiradas: 86.73804441052472% probabilidad de al menos 1 victoria\n",
      "200 tiradas: 98.24120533942786% probabilidad de al menos 1 victoria\n",
      "\n",
      "p = 10\n",
      "1 tiradas: 10.0% probabilidad de al menos 1 victoria\n",
      "2 tiradas: 19.0% probabilidad de al menos 1 victoria\n",
      "4 tiradas: 34.39% probabilidad de al menos 1 victoria\n",
      "10 tiradas: 65.13215598999999% probabilidad de al menos 1 victoria\n",
      "25 tiradas: 92.82102012308148% probabilidad de al menos 1 victoria\n",
      "50 tiradas: 99.484622479268% probabilidad de al menos 1 victoria\n",
      "100 tiradas: 99.99734386011123% probabilidad de al menos 1 victoria\n",
      "200 tiradas: 99.99999992944922% probabilidad de al menos 1 victoria\n",
      "\n",
      "p = 25\n",
      "1 tiradas: 25.0% probabilidad de al menos 1 victoria\n",
      "2 tiradas: 43.75% probabilidad de al menos 1 victoria\n",
      "4 tiradas: 68.359375% probabilidad de al menos 1 victoria\n",
      "10 tiradas: 94.36864852905273% probabilidad de al menos 1 victoria\n",
      "25 tiradas: 99.9247456541835% probabilidad de al menos 1 victoria\n",
      "50 tiradas: 99.99994336783433% probabilidad de al menos 1 victoria\n",
      "100 tiradas: 99.99999999996795% probabilidad de al menos 1 victoria\n",
      "200 tiradas: 100.00000000000003% probabilidad de al menos 1 victoria\n",
      "\n",
      "p = 50\n",
      "1 tiradas: 50.0% probabilidad de al menos 1 victoria\n",
      "2 tiradas: 75.0% probabilidad de al menos 1 victoria\n",
      "4 tiradas: 93.75% probabilidad de al menos 1 victoria\n",
      "10 tiradas: 99.90234375% probabilidad de al menos 1 victoria\n",
      "25 tiradas: 99.99999701976776% probabilidad de al menos 1 victoria\n",
      "50 tiradas: 99.99999999999991% probabilidad de al menos 1 victoria\n",
      "100 tiradas: 100.00000000000003% probabilidad de al menos 1 victoria\n",
      "200 tiradas: 100.0% probabilidad de al menos 1 victoria\n",
      "\n",
      "p = 75\n",
      "1 tiradas: 75.0% probabilidad de al menos 1 victoria\n",
      "2 tiradas: 93.75% probabilidad de al menos 1 victoria\n",
      "4 tiradas: 99.609375% probabilidad de al menos 1 victoria\n",
      "10 tiradas: 99.99990463256836% probabilidad de al menos 1 victoria\n",
      "25 tiradas: 99.99999999999991% probabilidad de al menos 1 victoria\n",
      "50 tiradas: 100.0% probabilidad de al menos 1 victoria\n",
      "100 tiradas: 100.0% probabilidad de al menos 1 victoria\n",
      "200 tiradas: 99.99999999999997% probabilidad de al menos 1 victoria\n"
     ]
    }
   ],
   "source": [
    "probabilidades = [2,10,25,50,75]\n",
    "for i,lista in enumerate(Lista):\n",
    "    print(\"\\np = {}\".format(probabilidades[i]))\n",
    "    for tiradas in [1,2,4,10,25,50,100,200]:\n",
    "        p = lista[tiradas]\n",
    "        print(\"{} tiradas: {}% probabilidad de al menos 1 victoria\".format(tiradas,p*100))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 121,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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XITTsQAAu/D6rP767no9+MkmTctN0r8vmsROlQAkURihG63d1qDrn/taW6qb9bNjh+rGY/IVEszC1gAQaLw+qwc/2aR7P9igxKgwPXbpFB07kpmmANDX1Le69bPZS/Xx+kr9aHq2/nDaaDamAbAThRG6xevLSvWb/6xUTESInrvqEB2al+x0JACAA7ZWt+iWOcu0uKhWp4zN0J9/MEaJ0WFOxwIA7KG22aXzHv5ShZXNuvPMMfrR9BynIwHoYyiM8L24vT7d/fY6PfbZZk3NTdT9F07icgMACEDWWr20uER3vLFaQUFG9543QWdMGDyCztoAACAASURBVChjWKsIAPqapnaPLntykbZUt+ipK6bpsPwUpyMB6IMojHDQKhvbdcPzX2vh5hpdNiNXvz1lpEK5BA0AAk51U7tufWWl3luzXYcOSdbfzh2vQQmsXwcAfVG7x6sfP7NEq0rr9eBFkymLAHwnCiMclNVl9brqqcWqbXHpH+eN15kTM52OBABwwH/XbdevXl6hhlaPbjtlpK44bDA7oAFAH+X1Wd384jJ9VlClv50znp2MAewThREO2Lury3XT7GVKiAply00ACFAer0/3vLteD88v1MiMOD131QQNHxDrdCwAwHew1uq211Zq3spy3XbKSJ09mRO+APaNwghdZq3Vg58U6p5312lcZoIeuXgy6xUBQACqaGjTDS8s1Veba3TJoTn67SkjFR4S7HQsAMA+/PXd9Xrhq2JdPzNPVx0xxOk4APoBCiN0SbvHq1tfWalXvi7VaeMH6q9nj1NEKAcHABBoFhZW6/rnl6q53aP7zp+gMyYMcjoSAGA/HplfqH9/vEkXTs/WL44f7nQcAP0EhRH2q6HNrWufXqIvC6t183HD9NNj89n1BgACjLVWj3xaqL+8s145SVF6/urpGpbOJWgA0NfNWVysO+et1SljM/SnM8bwezyALqMwwj6V17fpsie+UkFFE4tbA0CAamhz61cvrdA7q8t18tgB+stZ4xQbEep0LADAfry7uly/+c8KHTE0Rf933ngFsykBgANAYYTvtGF7oy57/Cs1tHn0xOVTdcTQVKcjAQB62bryBl337NfaWtOi204ZqSsPH8zZaQDoB77YVKUbX1iqcZkJevCiyaw1B+CAURhhr77aXKOrnlqk8NBgvXjtIRo9kJ3QACDQzFu5TbfMWaa4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\n",
      "text/plain": [
       "<Figure size 1440x720 with 5 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize=(20,10))\n",
    "for i in range(len(Lista)):\n",
    "    plt.subplot(2,3,i+1)\n",
    "    plt.plot(Lista[i][:200])\n",
    "    plt.title(\"p = {}\".format(probabilidades[i]))\n",
    "    if i == 1:\n",
    "        plt.xlim(0,40)\n",
    "    if i == 2:\n",
    "        plt.xlim(0,20)\n",
    "    if i in (3,4):\n",
    "        plt.xlim(0,5)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "p = 1 completado\n",
      "p = 2 completado\n"
     ]
    }
   ],
   "source": [
    "Lista = []\n",
    "probabilidades = [1,2,3,5,10,15,20]\n",
    "for p in probabilidades:\n",
    "    lista = []\n",
    "    for tiradas in range(1,501):\n",
    "        lista.append(P_actualizado_actualizado(p,tiradas))\n",
    "    print(\"p = {} completado\".format(p))\n",
    "    Lista.append(lista)\n",
    "\n",
    "for i,lista in enumerate(Lista):\n",
    "    print(\"\\np = {}\".format(probabilidades[i]))\n",
    "    for tiradas in [1,2,4,10,25,50,100,200]:\n",
    "        p = lista[tiradas-1]\n",
    "        print(\"{} tiradas: {}% probabilidad de al menos 1 victoria\".format(tiradas,p*100))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Voy a trasponer: para cada nº de tiradas, ver la P en función de cada p:**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(500, 7)"
      ]
     },
     "execution_count": 38,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "Lista_array = np.array(Lista)\n",
    "Lista_array_t = np.transpose(Lista_array)\n",
    "Lista_array_t.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[1, 2, 3, 5, 10, 15, 20]"
      ]
     },
     "execution_count": 39,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "probabilidades"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "\n",
      " 1 tiradas\n",
      "p = 1: 1.0% de probabilidad de al menos 1 victoria\n",
      "p = 2: 1.9999999999999998% de probabilidad de al menos 1 victoria\n",
      "p = 3: 3.0% de probabilidad de al menos 1 victoria\n",
      "p = 5: 5.0% de probabilidad de al menos 1 victoria\n",
      "p = 10: 10.0% de probabilidad de al menos 1 victoria\n",
      "p = 15: 15.0% de probabilidad de al menos 1 victoria\n",
      "p = 20: 20.0% de probabilidad de al menos 1 victoria\n",
      "\n",
      " 2 tiradas\n",
      "p = 1: 1.9900000000000002% de probabilidad de al menos 1 victoria\n",
      "p = 2: 3.959999999999999% de probabilidad de al menos 1 victoria\n",
      "p = 3: 5.91% de probabilidad de al menos 1 victoria\n",
      "p = 5: 9.749999999999998% de probabilidad de al menos 1 victoria\n",
      "p = 10: 19.0% de probabilidad de al menos 1 victoria\n",
      "p = 15: 27.750000000000004% de probabilidad de al menos 1 victoria\n",
      "p = 20: 36.00000000000001% de probabilidad de al menos 1 victoria\n",
      "\n",
      " 4 tiradas\n",
      "p = 1: 3.940399% de probabilidad de al menos 1 victoria\n",
      "p = 2: 7.763183999999996% de probabilidad de al menos 1 victoria\n",
      "p = 3: 11.470718999999999% de probabilidad de al menos 1 victoria\n",
      "p = 5: 18.549374999999998% de probabilidad de al menos 1 victoria\n",
      "p = 10: 34.38999999999999% de probabilidad de al menos 1 victoria\n",
      "p = 15: 47.79937499999999% de probabilidad de al menos 1 victoria\n",
      "p = 20: 59.04000000000001% de probabilidad de al menos 1 victoria\n",
      "\n",
      " 10 tiradas\n",
      "p = 1: 9.561792499119552% de probabilidad de al menos 1 victoria\n",
      "p = 2: 18.292719311245303% de probabilidad de al menos 1 victoria\n",
      "p = 3: 26.257587310507173% de probabilidad de al menos 1 victoria\n",
      "p = 5: 40.12630607616209% de probabilidad de al menos 1 victoria\n",
      "p = 10: 65.13215599% de probabilidad de al menos 1 victoria\n",
      "p = 15: 80.31255956592771% de probabilidad de al menos 1 victoria\n",
      "p = 20: 89.26258176000003% de probabilidad de al menos 1 victoria\n",
      "\n",
      " 25 tiradas\n",
      "p = 1: 22.21786406008532% de probabilidad de al menos 1 victoria\n",
      "p = 2: 39.65352702211029% de probabilidad de al menos 1 victoria\n",
      "p = 3: 53.30252947456279% de probabilidad de al menos 1 victoria\n",
      "p = 5: 72.26104268781651% de probabilidad de al menos 1 victoria\n",
      "p = 10: 92.82102012308152% de probabilidad de al menos 1 victoria\n",
      "p = 15: 98.28021901477918% de probabilidad de al menos 1 victoria\n",
      "p = 20: 99.62221068137058% de probabilidad de al menos 1 victoria\n",
      "\n",
      " 100 tiradas\n",
      "p = 1: 63.39676587267699% de probabilidad de al menos 1 victoria\n",
      "p = 2: 86.73804441052452% de probabilidad de al menos 1 victoria\n",
      "p = 3: 95.24474920745915% de probabilidad de al menos 1 victoria\n",
      "p = 5: 99.40794707796613% de probabilidad de al menos 1 victoria\n",
      "p = 10: 99.99734386011143% de probabilidad de al menos 1 victoria\n",
      "p = 15: 99.9999912523262% de probabilidad de al menos 1 victoria\n",
      "p = 20: 99.99999997963018% de probabilidad de al menos 1 victoria\n",
      "\n",
      " 200 tiradas\n",
      "p = 1: 86.60203251420369% de probabilidad de al menos 1 victoria\n",
      "p = 2: 98.24120533942745% de probabilidad de al menos 1 victoria\n",
      "p = 3: 99.7738758989999% de probabilidad de al menos 1 victoria\n",
      "p = 5: 99.99649473337413% de probabilidad de al menos 1 victoria\n",
      "p = 10: 99.9999999294496% de probabilidad de al menos 1 victoria\n",
      "p = 15: 99.99999999999893% de probabilidad de al menos 1 victoria\n",
      "p = 20: 100.0000000000011% de probabilidad de al menos 1 victoria\n"
     ]
    }
   ],
   "source": [
    "tiradas = [1,2,4,10,25,100,200]\n",
    "for tirada in tiradas:\n",
    "    print(\"\\n {} tiradas\".format(tirada))\n",
    "    for col,P in enumerate(Lista_array_t[tirada-1]):\n",
    "        print(\"p = {}: {}% de probabilidad de al menos 1 victoria\".format(probabilidades[col],P*100))"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
