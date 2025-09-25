import React from 'react';

interface ControlsProps {
  onReset: () => void;
  isSimulating: boolean;
  waterLevel: number;
  velocity: number;
  tubeLevel: number;
  onTubeLevelChange: (value: number) => void;
  tubeHorizontalLength: number;
  onTubeHorizontalLengthChange: (value: number) => void;
  tubeVerticalLength: number;
  onTubeVerticalLengthChange: (value: number) => void;
}

const Button: React.FC<React.ButtonHTMLAttributes<HTMLButtonElement>> = ({ children, ...props }) => {
  return (
    <button
      {...props}
      className="px-6 py-3 text-lg font-semibold rounded-lg shadow-md transition-all duration-300 focus:outline-none focus:ring-4 disabled:opacity-50 disabled:cursor-not-allowed"
    >
      {children}
    </button>
  );
};

const Slider: React.FC<{label: string, value: number, min: number, max: number, onChange: (e: React.ChangeEvent<HTMLInputElement>) => void}> = 
({ label, value, min, max, onChange }) => {
  return (
     <div className="w-full flex items-center space-x-4">
        <label className="w-36 text-sm font-medium text-slate-300">{label}</label>
        <input 
            type="range"
            min={min}
            max={max}
            value={value}
            onChange={onChange}
            className="w-full h-2 bg-slate-700 rounded-lg appearance-none cursor-pointer"
        />
        <span className="w-12 text-center font-mono text-cyan-300">{value}</span>
    </div>
  )
}

const Controls: React.FC<ControlsProps> = ({ 
  onReset, 
  isSimulating, 
  waterLevel,
  velocity,
  tubeLevel,
  onTubeLevelChange,
  tubeHorizontalLength,
  onTubeHorizontalLengthChange,
  tubeVerticalLength,
  onTubeVerticalLengthChange
}) => {
  const isBottleEmpty = waterLevel <= 0;

  return (
    <div className="mt-8 flex flex-col items-center space-y-6 w-full max-w-md p-6 bg-slate-800/50 rounded-xl border border-slate-700">
        
        {/* --- Water Level Indicator --- */}
        <div className="w-full flex flex-col items-center">
            <div className="w-full bg-slate-700 rounded-full h-4 border-2 border-slate-500">
                <div 
                    className="bg-gradient-to-r from-blue-500 to-cyan-400 h-full rounded-full transition-all duration-200 ease-linear" 
                    style={{ width: `${waterLevel}%` }}
                ></div>
            </div>
            <p className="mt-2 text-xl font-mono text-cyan-300 w-24 text-center">{waterLevel.toFixed(1)}%</p>
        </div>

        {/* --- Tube Configuration --- */}
        <div className="w-full flex flex-col items-center space-y-4 pt-4 border-t border-slate-700">
            <h3 className="text-lg font-semibold text-slate-200 mb-2">Tube Configuration</h3>
            <Slider 
                label="Высота выходного отверстия (%)"
                value={tubeLevel}
                min={10}
                max={90}
                onChange={(e) => onTubeLevelChange(Number(e.target.value))}
            />
            <Slider 
                label="Горизонтальная длина (px)"
                value={tubeHorizontalLength}
                min={20}
                max={150}
                onChange={(e) => onTubeHorizontalLengthChange(Number(e.target.value))}
            />
             <Slider 
                label="Вертикальное падение (px)"
                value={tubeVerticalLength}
                min={0}
                max={150}
                onChange={(e) => onTubeVerticalLengthChange(Number(e.target.value))}
            />
        </div>

        {/* --- Simulation Data Table --- */}
        <div className="w-full flex flex-col items-center space-y-4 pt-4 border-t border-slate-700">
            <h3 className="text-lg font-semibold text-slate-200 mb-2">Данные симуляции</h3>
            <table className="w-full text-sm text-left border-collapse">
                <thead className="text-xs text-slate-400 uppercase">
                    <tr>
                        <th className="p-2 border border-slate-600">Параметр</th>
                        <th className="p-2 border border-slate-600 text-right">Значение</th>
                    </tr>
                </thead>
                <tbody>
                    <tr className="bg-slate-800/50">
                        <td className="p-2 border border-slate-600 font-medium text-slate-300">Скорость воды (м/с)</td>
                        <td className="p-2 border border-slate-600 text-right font-mono text-cyan-300">{velocity.toFixed(2)}</td>
                    </tr>
                </tbody>
            </table>
        </div>


        {/* --- Main Controls --- */}
        <div className="flex justify-center pt-4">
            <Button
                onClick={onReset}
                disabled={!isSimulating && isBottleEmpty && tubeLevel === 64 && tubeHorizontalLength === 80 && tubeVerticalLength === 80}
                className="bg-slate-600 text-slate-200 hover:bg-slate-500 focus:ring-slate-400/50"
            >
                Сброс
            </Button>
        </div>
    </div>
  );
};

export default Controls;# Water-bottle-simulator
