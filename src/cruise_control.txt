
P = tf(1, [0.5, 1, 0])  % Engine time lag + step input

rlocus(P) % Find root locus all possible values of close loop poles by changing K

K = 0.7; % Gain

cltf = feedback(K*P, 1)
         
step(cltf) 
impulse(cltf)
bode(cltf)

% Lead Compensator - multiply by transfer function
l_c = tf([1, 2], [1, 8]) % cltf = feedback(l_c*K*P, 1)


% Control System Designer > Edit architecture > select plant from workspace
% 
% Second order underdamped transient step response 
damp_ratio = 0.829; % Read this from control system designer, bottom right
nat_freq = 1.21; % Read this from control system designer, bottom right

% Two percent settling time 
T_s = 4/(damp_ratio*nat_freq)

% Damped Frequency
damp_freq = nat_freq*(sqrt(1-damp_ratio^2))

% Peak Time
T_p = pi/(damp_freq)

% Percent Overshoot
P_O = 100*exp((-damp_ratio*pi)/(sqrt(1-damp_ratio^2)))


% Damp ratio to give Percent Overshoot
desired_p_o = 0.9496; 
damp_ratio = ((-log(desired_p_o/100))/(sqrt(pi^2 + (log(desired_p_o/100))^2)))

