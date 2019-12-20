function varargout = project(varargin)
% PROJECT MATLAB code for project.fig
%      PROJECT, by itself, creates a new PROJECT or raises the existing
%      singleton*.
%
%      H = PROJECT returns the handle to a new PROJECT or the handle to
%      the existing singleton*.
%
%      PROJECT('CALLBACK',hObject,eventData,handles,...) calls the local
%      function named CALLBACK in PROJECT.M with the given input arguments.
%
%      PROJECT('Property','Value',...) creates a new PROJECT or raises the
%      existing singleton*.  Starting from the left, property value pairs are
%      applied to the GUI before project_OpeningFcn gets called.  An
%      unrecognized property name or invalid value makes property application
%      stop.  All inputs are passed to project_OpeningFcn via varargin.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help project

% Last Modified by GUIDE v2.5 05-Dec-2019 17:21:50

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @project_OpeningFcn, ...
                   'gui_OutputFcn',  @project_OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT


% --- Executes just before project is made visible.
function project_OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   command line arguments to project (see VARARGIN)



clc;


handles.Kv = 3;
handles.alpha_Sv = 0.5;
handles.alpha_Ev = 0.5;
handles.betav = 0.5;
handles.K_Ev = 3;

% [y,Fs] = audioread('samplesound.mp3');
load handel.mat;
handles.myy = y;
handles.myFs = Fs;
%-------------------Low Pass Filter------------------------

handles.L_scalev = (1-handles.alpha_Sv)/2;
handles.L_bv = [1 1];
handles.b_Lv = handles.L_scalev*handles.L_bv;
handles.av = [1 -handles.alpha_Sv];
handles.output_Lv = filter(handles.b_Lv,handles.av,y);
axes(handles.axesO);
plot(1:length(y),y)

[hL wL] = freqz(handles.b_Lv,handles.av,'whole');
handles.vectorLv = hL;


%-----------------High Pass Filter----------------------------
handles.H_scalev = (1+handles.alpha_Sv)/2;
handles.H_bv = [1 -1];
handles.b_Hv = handles.H_scalev*handles.H_bv;
handles.output_Hv = filter(handles.b_Hv,handles.av,y);
[hH wH] = freqz(handles.b_Hv,handles.av,'whole');
handles.vectorHv = hH;


%---------------------Shelving Low Frequency Filter---------------------
handles.g_Lv = handles.Kv*handles.vectorLv + handles.vectorHv;
handles.u_Lv = conv(y,abs(handles.g_Lv));
axes(handles.axesSL);
plot(1:length(handles.u_Lv),handles.u_Lv);


%---------------------- Shelving High Frequency Filter------------------------------------
handles.g_Hv = handles.vectorLv + handles.Kv*handles.vectorHv;
handles.u_Hv = conv(y,abs(handles.g_Hv));
axes(handles.axesSH);
plot(1:length(handles.u_Hv),handles.u_Hv);










%----------------------------Band Pass Filter------------------


handles.scale_Pv = (1 - handles.alpha_Ev)/2;
handles.P_bv = [1 0 -1];
handles.b_Pv = handles.scale_Pv * handles.P_bv;
handles.a_Ev = [1 (-handles.betav*(handles.alpha_Ev+1)) handles.alpha_Ev];

handles.output_Pv = filter(handles.b_Pv, handles.a_Ev, y);

[h_P w_P] = freqz(handles.b_Pv, handles.a_Ev, 'whole');
handles.vectorPv = h_P;

%----------------------------Band Stop Filter------------------
handles.scale_Sv = (1 + handles.alpha_Ev)/2;
handles.S_bv = [1 -2*handles.betav 1];
handles.b_Sv = handles.scale_Sv * handles.S_bv;

handles.output_Sv = filter(handles.b_Sv, handles.a_Ev, y);

[h_S w_S] = freqz(handles.b_Sv, handles.a_Ev, 'whole');
handles.vectorSv = h_S;
%----------------------------Equilizer-------------------------

handles.g_Ev = handles.K_Ev*handles.vectorPv + handles.vectorSv;
handles.u_Ev = conv(y,abs(handles.g_Ev));
axes(handles.axesE);
plot(1:length(handles.u_Ev),handles.u_Ev);



% Choose default command line output for project
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes project wait for user response (see UIRESUME)
% uiwait(handles.figure1);


% --- Outputs from this function are returned to the command line.
function varargout = project_OutputFcn(hObject, eventdata, handles) 
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;


% --- Executes on slider movement.
function slider1_Callback(hObject, eventdata, handles)
% hObject    handle to slider1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider


% --- Executes during object creation, after setting all properties.
function slider1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to slider1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on button press in PlotO.
function PlotO_Callback(hObject, eventdata, handles)
% hObject    handle to PlotO (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)


% --- Executes on button press in plotSH.
function plotSH_Callback(hObject, eventdata, handles)
% hObject    handle to plotSH (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
axes(handles.axesSH)
plot(1:length(handles.u_Hv),handles.u_Hv);


% --- Executes on button press in plotSL.
function plotSL_Callback(hObject, eventdata, handles)
% hObject    handle to plotSL (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
axes(handles.axesSL);
plot(1:length(handles.u_Lv),handles.u_Lv);


% --- Executes on button press in plotE.
function plotE_Callback(hObject, eventdata, handles)
% hObject    handle to plotE (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
axes(handles.axesE);
plot(1:length(handles.u_Ev),handles.u_Ev);

% --- Executes on button press in playO.
function playO_Callback(hObject, eventdata, handles)
% hObject    handle to playO (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

sound(handles.myy,handles.myFs);


% --- Executes on button press in playSH.
function playSH_Callback(hObject, eventdata, handles)
% hObject    handle to playSH (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
sound(handles.u_Hv,handles.myFs);

% --- Executes on button press in playSL.
function playSL_Callback(hObject, eventdata, handles)
% hObject    handle to playSL (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
sound(handles.u_Lv,handles.myFs);

% --- Executes on button press in playE.
function playE_Callback(hObject, eventdata, handles)
% hObject    handle to playE (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
sound(handles.u_Ev,handles.myFs);

% --- Executes on slider movement.
function alpha_Callback(hObject, eventdata, handles)
% hObject    handle to alpha_S (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

%-------------------Low Pass Filter------------------------
set(handles.text7,'String',get(handles.alpha_S,'value'));
handles.aplha_Sv = get(handles.alpha_S,'value');
guidata(hObject,handles);
handles.L_scalev = (1-handles.alpha_Sv)/2;
guidata(hObject,handles);
handles.L_bv = [1 1];
guidata(hObject,handles);
handles.b_Lv = handles.L_scalev*handles.L_bv;
guidata(hObject,handles);
handles.av = [1 -handles.alpha_Sv];
guidata(hObject,handles);
handles.output_Lv = filter(handles.b_Lv,handles.av,handles.myy);
guidata(hObject,handles);

[hL wL] = freqz(handles.b_Lv,handles.av,'whole');
guidata(hObject,handles);
handles.vectorLv = hL;
guidata(hObject,handles);


%-----------------High Pass Filter----------------------------
handles.H_scalev = (1+handles.alpha_Sv)/2;
guidata(hObject,handles);
handles.H_bv = [1 -1];
guidata(hObject,handles);
handles.b_Hv = handles.H_scalev*handles.H_bv;
guidata(hObject,handles);
handles.output_Hv = filter(handles.b_Hv,handles.av,handles.myy);
guidata(hObject,handles);
[hH wH] = freqz(handles.b_Hv,handles.av,'whole');
guidata(hObject,handles);
handles.vectorHv = hH;
guidata(hObject,handles);

%---------------------Shelving Low Frequency Filter---------------------
handles.g_Lv = handles.Kv*handles.vectorLv + handles.vectorHv;
guidata(hObject,handles);
handles.u_Lv = conv(handles.myy,abs(handles.g_Lv));
guidata(hObject,handles);

%---------------------- Shelving High Frequency Filter------------------------------------
handles.g_Hv = handles.vectorLv + handles.Kv*handles.vectorHv;
guidata(hObject,handles);
handles.u_Hv = conv(handles.myy,abs(handles.g_Hv));
guidata(hObject,handles);



% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider


% --- Executes during object creation, after setting all properties.
function alpha_S_CreateFcn(hObject, eventdata, handles)
% hObject    handle to alpha_S (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on slider movement.
function K_Callback(hObject, eventdata, handles)
% hObject    handle to K (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

set(handles.text9,'String',get(handles.K,'value'));
handles.Kv = get(handles.K,'value');
guidata(hObject,handles);
%---------------------Shelving Low Frequency Filter---------------------
handles.g_Lv = handles.Kv*handles.vectorLv + handles.vectorHv;
guidata(hObject,handles);
handles.u_Lv = conv(handles.myy,abs(handles.g_Lv));
guidata(hObject,handles);

%---------------------- Shelving High Frequency Filter------------------------------------
handles.g_Hv = handles.vectorLv + handles.Kv*handles.vectorHv;
guidata(hObject,handles);
handles.u_Hv = conv(handles.myy,abs(handles.g_Hv));
guidata(hObject,handles);

% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider


% --- Executes during object creation, after setting all properties.
function K_CreateFcn(hObject, eventdata, handles)
% hObject    handle to K (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on slider movement.
function alpha_E_Callback(hObject, eventdata, handles)
% hObject    handle to alpha_E (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

set(handles.text8,'String',get(handles.alpha_E,'value'));
handles.alpha_Ev = get(handles.alpha_E,'value');
guidata(hObject,handles)
handles.scale_Pv = (1 - handles.alpha_Ev)/2;
guidata(hObject,handles)
handles.P_bv = [1 0 -1];
guidata(hObject,handles)
handles.b_Pv = handles.scale_Pv * handles.P_bv;
guidata(hObject,handles)
handles.a_Ev = [1 (-handles.betav*(handles.alpha_Ev+1)) handles.alpha_Ev];
guidata(hObject,handles)
handles.output_Pv = filter(handles.b_Pv, handles.a_Ev, handles.myy);
guidata(hObject,handles)
[h_P w_P] = freqz(handles.b_Pv, handles.a_Ev, 'whole');
handles.vectorPv = h_P;
guidata(hObject,handles)
%----------------------------Band Stop Filter------------------
handles.scale_Sv = (1 + handles.alpha_Ev)/2;
guidata(hObject,handles)
handles.S_bv = [1 -2*handles.betav 1];
guidata(hObject,handles)
handles.b_Sv = handles.scale_Sv * handles.S_bv;
guidata(hObject,handles)
handles.output_Sv = filter(handles.b_Sv, handles.a_Ev, handles.myy);
guidata(hObject,handles)
[h_S w_S] = freqz(handles.b_Sv, handles.a_Ev, 'whole');
handles.vectorSv = h_S;
guidata(hObject,handles)
%----------------------------Equilizer-------------------------

handles.g_Ev = handles.K_Ev*handles.vectorPv + handles.vectorSv;
guidata(hObject,handles)
handles.u_Ev = conv(handles.myy,abs(handles.g_Ev));
guidata(hObject,handles)

% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider


% --- Executes during object creation, after setting all properties.
function alpha_E_CreateFcn(hObject, eventdata, handles)
% hObject    handle to alpha_E (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on slider movement.
function K_E_Callback(hObject, eventdata, handles)
% hObject    handle to K_E (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
num = get(handles.K_E,'value');
handles.K_Ev =num ;
set(handles.text10,'String',handles.K_Ev);

handles.g_Ev = (num*handles.vectorPv) + handles.vectorSv;
guidata(hObject,handles)
handles.u_Ev = conv(handles.myy,abs(handles.g_Ev));
guidata(hObject,handles)
% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider


% --- Executes during object creation, after setting all properties.
function K_E_CreateFcn(hObject, eventdata, handles)
% hObject    handle to K_E (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on slider movement.
function BETA_Callback(hObject, eventdata, handles)
% hObject    handle to text69 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

set(handles.text11,'String',get(handles.BETA,'value'));
handles.betav = get(handles.BETA,'value');
guidata(hObject,handles)
%----------------------------Band Pass Filter------------------


handles.scale_Pv = (1 - handles.alpha_Ev)/2;
guidata(hObject,handles)
handles.P_bv = [1 0 -1];
guidata(hObject,handles)
handles.b_Pv = handles.scale_Pv * handles.P_bv;
guidata(hObject,handles)
handles.a_Ev = [1 (-handles.betav*(handles.alpha_Ev+1)) handles.alpha_Ev];
guidata(hObject,handles)
handles.output_Pv = filter(handles.b_Pv, handles.a_Ev, handles.myy);
guidata(hObject,handles)
[h_P w_P] = freqz(handles.b_Pv, handles.a_Ev, 'whole');
handles.vectorPv = h_P;
guidata(hObject,handles)
%----------------------------Band Stop Filter------------------
handles.scale_Sv = (1 + handles.alpha_Ev)/2;
guidata(hObject,handles)
handles.S_bv = [1 -2*handles.betav 1];
guidata(hObject,handles)
handles.b_Sv = handles.scale_Sv * handles.S_bv;
guidata(hObject,handles)
handles.output_Sv = filter(handles.b_Sv, handles.a_Ev, handles.myy);
guidata(hObject,handles)
[h_S w_S] = freqz(handles.b_Sv, handles.a_Ev, 'whole');
handles.vectorSv = h_S;
guidata(hObject,handles)
%----------------------------Equilizer-------------------------

handles.g_Ev = handles.K_Ev*handles.vectorPv + handles.vectorSv;
guidata(hObject,handles)
handles.u_Ev = conv(handles.myy,abs(handles.g_Ev));
guidata(hObject,handles)
% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider


% --- Executes during object creation, after setting all properties.
function text69_CreateFcn(hObject, eventdata, handles)
% hObject    handle to text69 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes during object creation, after setting all properties.
function BETA_CreateFcn(hObject, eventdata, handles)
% hObject    handle to BETA (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on slider movement.
function alpha_S_Callback(hObject, eventdata, handles)
% hObject    handle to alpha_S (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

set(handles.text7,'String',get(handles.alpha_S,'value'));
handles.alpha_Sv = get(handles.alpha_S,'value');
guidata(hObject,handles);
handles.L_scalev = (1-handles.alpha_Sv)/2;
guidata(hObject,handles);
handles.L_bv = [1 1];
guidata(hObject,handles);
handles.b_Lv = handles.L_scalev*handles.L_bv;
guidata(hObject,handles);
handles.av = [1 -handles.alpha_Sv];
guidata(hObject,handles);
handles.output_Lv = filter(handles.b_Lv,handles.av,handles.myy);
guidata(hObject,handles);

[hL wL] = freqz(handles.b_Lv,handles.av,'whole');
guidata(hObject,handles);
handles.vectorLv = hL;
guidata(hObject,handles);


%-----------------High Pass Filter----------------------------
handles.H_scalev = (1+handles.alpha_Sv)/2;
guidata(hObject,handles);
handles.H_bv = [1 -1];
guidata(hObject,handles);
handles.b_Hv = handles.H_scalev*handles.H_bv;
guidata(hObject,handles);
handles.output_Hv = filter(handles.b_Hv,handles.av,handles.myy);
guidata(hObject,handles);
[hH wH] = freqz(handles.b_Hv,handles.av,'whole');
guidata(hObject,handles);
handles.vectorHv = hH;
guidata(hObject,handles);

%---------------------Shelving Low Frequency Filter---------------------
handles.g_Lv = handles.Kv*handles.vectorLv + handles.vectorHv;
guidata(hObject,handles);
handles.u_Lv = conv(handles.myy,abs(handles.g_Lv));
guidata(hObject,handles);

%---------------------- Shelving High Frequency Filter------------------------------------
handles.g_Hv = handles.vectorLv + handles.Kv*handles.vectorHv;
guidata(hObject,handles);
handles.u_Hv = conv(handles.myy,abs(handles.g_Hv));
guidata(hObject,handles);

% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider
