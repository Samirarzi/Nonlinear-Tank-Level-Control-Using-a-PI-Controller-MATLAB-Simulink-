# Nonlinear-Tank-Level-Control-Using-a-PI-Controller-MATLAB-Simulink-
Overview  This project implements and simulates a nonlinear liquid-level control of a single tank in MATLAB/Simulink. The plant is nonlinear because the outflow depends on the square root of the liquid height. A PI controller is used in a feedback loop to regulate the level to a desired setpoint.

Physical nonlinear model

Let:

 = liquid level (m)

 = inflow rate (m³/s) — control input

 = cross-sectional area of the tank (m²)

 = discharge constant (units such that outflow has m³/s)


The nonlinear mass balance:

\frac{dh}{dt} = \frac{1}{A}\big(q_\text{in} - q_\text{out}\big)
\qquad\text{with}\qquad
q_\text{out} = k\sqrt{h}.

Thus the governing differential equation is

\boxed{\;\frac{dh}{dt}=\frac{1}{A}\big(q_\text{in} - k\sqrt{h}\big)\; }.


---

Equilibrium operating point

Let the equilibrium (steady) operating point be  satisfying:

0 = \frac{1}{A}\big(q_{in,0} - k\sqrt{h_0}\big)
\quad\Rightarrow\quad
q_{in,0} = k\sqrt{h_0}.


---

Linearization (small-signal model)

Define small deviations:

\Delta h = h - h_0,\qquad \Delta q = q_\text{in} - q_{in,0}.

Linearize  about :

k\sqrt{h} \approx k\sqrt{h_0} + \frac{k}{2\sqrt{h_0}}\Delta h.

Substitute into model and keep first-order terms:

\Delta\dot h = \frac{1}{A}\big(\Delta q - \tfrac{k}{2\sqrt{h_0}}\,\Delta h\big).

Define

a \triangleq \frac{k}{2A\sqrt{h_0}}.

\boxed{\;\Delta\dot h = -a\,\Delta h + \frac{1}{A}\,\Delta q\; }.


---

Transfer function (linearized plant)

Take Laplace transform (zero initial conditions):

s\,\Delta H(s) = -a\,\Delta H(s) + \frac{1}{A}\,\Delta Q(s).

\boxed{\;G(s) \;=\; \frac{1/A}{s+a} \;=\; \frac{1}{A(s+a)}\; }.

Key parameters:

DC gain .

Time constant .



---

Controller structure

Use a PI controller:

C(s)=K_p + \frac{K_i}{s} = \frac{K_p s + K_i}{s}.

Open-loop transfer:

L(s)=C(s)G(s)
= \frac{K_p s + K_i}{s}\cdot\frac{1}{A(s+a)}
= \frac{K_p s + K_i}{A\,s\,(s+a)}.

Closed-loop characteristic equation  leads to:

A\,s\,(s+a) + K_p s + K_i = 0.

Expand:

\boxed{\;A s^2 + (A a + K_p)\,s + K_i = 0\; }.

This is a second-order polynomial in  (because the controller introduces an integrator but the open-loop plant has one pole; the integrator cancels one  in numerator producing a second-order closed-loop denominator).


---

Controller design by pole placement (desired second-order)

Choose desirable closed-loop second-order form:

s^2 + 2\zeta\omega_n s + \omega_n^2 = 0.

Match coefficients with the plant polynomial divided by :

s^2 + \frac{A a + K_p}{A}\,s + \frac{K_i}{A} = 0.

Therefore:

\begin{aligned}
2\zeta\omega_n &= \frac{A a + K_p}{A} \\
\omega_n^2 &= \frac{K_i}{A}
\end{aligned}

\boxed{\;
\begin{aligned}
K_p &= 2\zeta\omega_n A - A a,\\[6pt]
K_i &= A\,\omega_n^2.
\end{aligned}
\; }

These formulas give direct PI gains to place closed-loop poles at desired .


---

Steady-state behavior

Because the controller contains an integrator, a step reference in level produces zero steady-state error (for constant references), assuming closed-loop stability.
