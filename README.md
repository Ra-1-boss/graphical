# graphical
% MATLAB script for graphical method in nonlinear circuit (diode example)
clear all;
close all;

% Parameters
Vs = 5;           % Supply voltage (V)
R = 1000;         % Resistor (Ohms)
Is = 1e-12;       % Diode reverse saturation current (A)
n = 1;            % Ideality factor
Vt = 0.025;       % Thermal voltage (V)

% Voltage range for plotting
Vd = 0:0.01:1;    % Diode voltage from 0 to 1 V (fine resolution)

% Diode I-V characteristic: I_D = Is * (exp(V_D / n*Vt) - 1)
Id_diode = Is * (exp(Vd / (n * Vt)) - 1);

% Load line: I_D = -V_D / R + Vs / R
Id_load = -Vd / R + Vs / R;

% Plotting
figure;
plot(Vd, Id_diode * 1000, 'b-', 'LineWidth', 2, 'DisplayName', 'Diode I-V Curve'); % Convert to mA
hold on;
plot(Vd, Id_load * 1000, 'r--', 'LineWidth', 2, 'DisplayName', 'Load Line'); % Convert to mA
grid on;
xlabel('Diode Voltage V_D (V)');
ylabel('Diode Current I_D (mA)');
title('Graphical Method for Diode Circuit');
legend('show');
hold off;

% Optional: Numerically find intersection point
% Define the difference function (diode curve - load line)
diff_func = @(Vd) Is * (exp(Vd / (n * Vt)) - 1) - (-Vd / R + Vs / R);
Vd_guess = 0.7; % Initial guess for diode voltage
Vd_op = fzero(diff_func, Vd_guess); % Find root
Id_op = Is * (exp(Vd_op / (n * Vt)) - 1); % Corresponding current

% Display operating point
fprintf('Operating Point: V_D = %.3f V, I_D = %.3f mA\n', Vd_op, Id_op * 1000);
