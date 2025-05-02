# My-project
Interactive dashboard for monthly sales data to improve KPI visibility and insights
import React from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { PieChart, Pie, Cell, Legend, ResponsiveContainer, Tooltip } from 'recharts';
import { SalesByCategory } from '@/services/salesData';

interface CategoryChartProps {
  data: SalesByCategory[];
}

const COLORS = ['#2D7DD2', '#45B7D1', '#26C485', '#F58A07', '#F45B69'];

const formatCurrency = (value: number) => {
  return `$${value.toLocaleString()}`;
};

const CategoryChart = ({ data }: CategoryChartProps) => {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Sales by Category</CardTitle>
      </CardHeader>
      <CardContent className="h-[300px]">
        <ResponsiveContainer width="100%" height="100%">
          <PieChart>
            <Pie
              data={data}
              cx="50%"
              cy="50%"
              labelLine={false}
              outerRadius={80}
              fill="#8884d8"
              dataKey="sales"
              nameKey="category"
              label={({ name, percent }) => `${name}: ${(percent * 100).toFixed(0)}%`}
            >
              {data.map((entry, index) => (
                <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
              ))}
            </Pie>
            <Legend />
            <Tooltip formatter={(value) => formatCurrency(Number(value))} />
          </PieChart>
        </ResponsiveContainer>
      </CardContent>
    </Card>
  );
};

export default CategoryChart;

import React from 'react';
import TimeFilter from './TimeFilter';
import { BarChart3 } from 'lucide-react';

interface HeaderProps {
  timePeriod: string;
  onTimeChange: (value: string) => void;
}

const Header = ({ timePeriod, onTimeChange }: HeaderProps) => {
  return (
    <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4">
      <div className="flex items-center">
        <BarChart3 className="h-8 w-8 text-dashboard-blue mr-3" />
        <h1 className="text-2xl font-bold">Sales Dashboard</h1>
      </div>
      <TimeFilter value={timePeriod} onValueChange={onTimeChange} />
    </div>
  );
};

export default Header;

import React from 'react';
import { Card, CardContent, CardFooter } from "@/components/ui/card";
import { Progress } from "@/components/ui/progress";

interface InfoCardProps {
  title: string;
  value: string;
  target?: number;
  subtitle?: string;
  className?: string;
}

const InfoCard = ({ title, value, target, subtitle, className }: InfoCardProps) => {
  return (
    <Card className={className}>
      <CardContent className="pt-6">
        <h3 className="text-sm font-medium text-muted-foreground">{title}</h3>
        <div className="mt-2 mb-1">
          <span className="text-3xl font-bold">{value}</span>
        </div>
        {subtitle && <p className="text-sm text-muted-foreground">{subtitle}</p>}
      </CardContent>
      {target !== undefined && (
        <CardFooter className="pt-0">
          <div className="w-full">
            <div className="flex justify-between text-sm mb-1">
              <span>Target Completion</span>
              <span>{target}%</span>
            </div>
            <Progress value={target} className="h-2" />
          </div>
        </CardFooter>
      )}
    </Card>
  );
};

import React from 'react';
import { Card, CardContent } from "@/components/ui/card";
import { ArrowUpIcon, ArrowDownIcon } from 'lucide-react';

interface KPICardProps {
  title: string;
  value: string | number;
  change?: number;
  icon?: React.ReactNode;
  className?: string;
}

const KPICard = ({ title, value, change, icon, className }: KPICardProps) => {
  const isPositiveChange = change && change > 0;
  
  return (
    <Card className={`overflow-hidden ${className}`}>
      <CardContent className="p-6">
        <div className="flex items-center justify-between">
          <div>
            <p className="text-sm font-medium text-muted-foreground">{title}</p>
            <h3 className="text-2xl font-bold mt-1">{value}</h3>
            
            {change !== undefined && (
              <div className="flex items-center mt-2">
                {isPositiveChange ? (
                  <ArrowUpIcon className="h-4 w-4 text-dashboard-green mr-1" />
                ) : (
                  <ArrowDownIcon className="h-4 w-4 text-dashboard-red mr-1" />
                )}
                <span className={`text-sm font-medium ${isPositiveChange ? 'text-dashboard-green' : 'text-dashboard-red'}`}>
                  {Math.abs(change).toFixed(1)}%
                </span>
              </div>
            )}
          </div>
          
          {icon && (
            <div className="text-dashboard-blue">
              {icon}
            </div>
          )}
        </div>
      </CardContent>
    </Card>
  );
};

export default KPICard;


import React from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { BarChart, Bar, XAxis, YAxis, Tooltip, ResponsiveContainer, CartesianGrid } from 'recharts';
import { MonthlySales } from '@/services/salesData';

interface MonthlyChartProps {
  data: MonthlySales[];
}

const formatCurrency = (value: number) => {
  return `$${value.toLocaleString()}`;
};

const MonthlyChart = ({ data }: MonthlyChartProps) => {
  return (
    <Card className="col-span-2">
      <CardHeader>
        <CardTitle>Monthly Sales</CardTitle>
      </CardHeader>
      <CardContent className="h-[300px]">
        <ResponsiveContainer width="100%" height="100%">
          <BarChart data={data} margin={{ top: 5, right: 30, left: 20, bottom: 5 }}>
            <CartesianGrid strokeDasharray="3 3" vertical={false} />
            <XAxis dataKey="month" />
            <YAxis tickFormatter={(value) => `$${value/1000}k`} />
            <Tooltip formatter={(value) => formatCurrency(Number(value))} />
            <Bar dataKey="sales" fill="#2D7DD2" radius={[4, 4, 0, 0]} />
          </BarChart>
        </ResponsiveContainer>
      </CardContent>
    </Card>
  );
};

export default MonthlyChart;
import React from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { BarChart, Bar, XAxis, YAxis, Tooltip, ResponsiveContainer, CartesianGrid } from 'recharts';
import { SalesByRegion } from '@/services/salesData';

interface RegionChartProps {
  data: SalesByRegion[];
}

const formatCurrency = (value: number) => {
  return `$${value.toLocaleString()}`;
};

const RegionChart = ({ data }: RegionChartProps) => {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Sales by Region</CardTitle>
      </CardHeader>
      <CardContent className="h-[300px]">
        <ResponsiveContainer width="100%" height="100%">
          <BarChart
            data={data}
            layout="vertical"
            margin={{ top: 5, right: 30, left: 50, bottom: 5 }}
          >
            <CartesianGrid strokeDasharray="3 3" horizontal={true} vertical={false} />
            <XAxis type="number" tickFormatter={(value) => `$${value/1000}k`} />
            <YAxis type="category" dataKey="region" />
            <Tooltip formatter={(value) => formatCurrency(Number(value))} />
            <Bar dataKey="sales" fill="#45B7D1" radius={[0, 4, 4, 0]} />
          </BarChart>
        </ResponsiveContainer>
      </CardContent>
    </Card>
  );
};

export default RegionChart;



import React from 'react';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';

interface TimeFilterProps {
  value: string;
  onValueChange: (value: string) => void;
}

const TimeFilter = ({ value, onValueChange }: TimeFilterProps) => {
  return (
    <div className="flex items-center space-x-2">
      <span className="text-sm font-medium">Time Period:</span>
      <Select value={value} onValueChange={onValueChange}>
        <SelectTrigger className="w-[180px]">
          <SelectValue placeholder="Select time period" />
        </SelectTrigger>
        <SelectContent>
          <SelectItem value="30d">Last 30 days</SelectItem>
          <SelectItem value="90d">Last 90 days</SelectItem>
          <SelectItem value="6m">Last 6 months</SelectItem>
          <SelectItem value="12m">Last 12 months</SelectItem>
          <SelectItem value="ytd">Year to date</SelectItem>
        </SelectContent>
      </Select>
    </div>
  );
};

export default TimeFilter;

import React from 'react';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';

interface TimeFilterProps {
  value: string;
  onValueChange: (value: string) => void;
}

const TimeFilter = ({ value, onValueChange }: TimeFilterProps) => {
  return (
    <div className="flex items-center space-x-2">
      <span className="text-sm font-medium">Time Period:</span>
      <Select value={value} onValueChange={onValueChange}>
        <SelectTrigger className="w-[180px]">
          <SelectValue placeholder="Select time period" />
        </SelectTrigger>
        <SelectContent>
          <SelectItem value="30d">Last 30 days</SelectItem>
          <SelectItem value="90d">Last 90 days</SelectItem>
          <SelectItem value="6m">Last 6 months</SelectItem>
          <SelectItem value="12m">Last 12 months</SelectItem>
          <SelectItem value="ytd">Year to date</SelectItem>
        </SelectContent>
      </Select>
    </div>
  );
};

export default TimeFilter;
export interface SalesData {
  id: number;
  date: string;
  amount: number;
  product: string;
  category: string;
  region: string;
  representative: string;
}

export interface SalesByCategory {
  category: string;
  sales: number;
}

export interface SalesByRegion {
  region: string;
  sales: number;
}

export interface KPIData {
  totalSales: number;
  averageSale: number;
  salesGrowth: number;
  topProduct: string;
  topRegion: string;
  targetCompletion: number;
}

export interface MonthlySales {
  month: string;
  sales: number;
}

// Generate mock sales data for the past 12 months
const generateMockSalesData = (): SalesData[] => {
  const regions = ['North', 'South', 'East', 'West', 'Central'];
  const categories = ['Electronics', 'Furniture', 'Clothing', 'Accessories', 'Home Goods'];
  const products = {
    Electronics: ['Laptop', 'Smartphone', 'Tablet', 'Headphones', 'Smart Watch'],
    Furniture: ['Sofa', 'Chair', 'Table', 'Desk', 'Bookshelf'],
    Clothing: ['T-Shirt', 'Jeans', 'Dress', 'Jacket', 'Shoes'],
    Accessories: ['Bag', 'Wallet', 'Sunglasses', 'Belt', 'Hat'],
    'Home Goods': ['Lamp', 'Rug', 'Curtains', 'Pillow', 'Vase']
  };
  const representatives = [
    'John Smith', 'Emma Johnson', 'Michael Brown', 'Sarah Davis', 
    'Robert Wilson', 'Linda Garcia', 'William Martinez', 'Jennifer Robinson'
  ];
  
  const data: SalesData[] = [];
  const now = new Date();
  const endDate = new Date();
  const startDate = new Date(now);
  startDate.setMonth(startDate.getMonth() - 12);
  
  let id = 1;
  
  // Generate between 500-600 sales records
  const numRecords = Math.floor(Math.random() * 100) + 500;
  
  for (let i = 0; i < numRecords; i++) {
    // Random date in the past 12 months
    const date = new Date(startDate.getTime() + Math.random() * (endDate.getTime() - startDate.getTime()));
    
    // Random category and product
    const category = categories[Math.floor(Math.random() * categories.length)];
    const product = products[category][Math.floor(Math.random() * products[category].length)];
    
    // Random amount between $50 and $2000
    const amount = Math.floor(Math.random() * 1950) + 50;
    
    // Random region and representative
    const region = regions[Math.floor(Math.random() * regions.length)];
    const representative = representatives[Math.floor(Math.random() * representatives.length)];
    
    data.push({
      id: id++,
      date: date.toISOString().split('T')[0],
      amount,
      product,
      category,
      region,
      representative
    });
  }
  
  return data;
};

// Cache the generated data
const mockSalesData = generateMockSalesData();

// Calculate KPIs from the mock data
export const getKPIData = (): KPIData => {
  const totalSales = mockSalesData.reduce((sum, sale) => sum + sale.amount, 0);
  const averageSale = totalSales / mockSalesData.length;
  
  // Group by product to find the top product
  const productSales: Record<string, number> = {};
  mockSalesData.forEach(sale => {
    productSales[sale.product] = (productSales[sale.product] || 0) + sale.amount;
  });
  const topProduct = Object.entries(productSales).sort((a, b) => b[1] - a[1])[0][0];
  
  // Group by region to find the top region
  const regionSales: Record<string, number> = {};
  mockSalesData.forEach(sale => {
    regionSales[sale.region] = (regionSales[sale.region] || 0) + sale.amount;
  });
  const topRegion = Object.entries(regionSales).sort((a, b) => b[1] - a[1])[0][0];
  
  // Calculate growth (comparing recent half to earlier half)
  const midPoint = Math.floor(mockSalesData.length / 2);
  const recentSales = mockSalesData.slice(midPoint).reduce((sum, sale) => sum + sale.amount, 0);
  const olderSales = mockSalesData.slice(0, midPoint).reduce((sum, sale) => sum + sale.amount, 0);
  const salesGrowth = ((recentSales - olderSales) / olderSales) * 100;
  
  // Random target completion between 70% and 110%
  const targetCompletion = Math.floor(Math.random() * 40) + 70;
  
  return {
    totalSales,
    averageSale,
    salesGrowth,
    topProduct,
    topRegion,
    targetCompletion
  };
};

// Get monthly sales data for the chart
export const getMonthlySalesData = (): MonthlySales[] => {
  const monthlySales: Record<string, number> = {};
  const months = [
    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 
    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
  ];
  
  // Initialize all months with zero
  const currentMonth = new Date().getMonth();
  for (let i = 0; i < 12; i++) {
    const monthIndex = (currentMonth - 11 + i) % 12;
    monthlySales[months[monthIndex]] = 0;
  }
  
  // Aggregate sales by month
  mockSalesData.forEach(sale => {
    const date = new Date(sale.date);
    const monthName = months[date.getMonth()];
    monthlySales[monthName] = (monthlySales[monthName] || 0) + sale.amount;
  });
  
  // Convert to array for chart
  return Object.entries(monthlySales).map(([month, sales]) => ({ month, sales }));
};

// Get sales by category data for the chart
export const getSalesByCategoryData = (): SalesByCategory[] => {
  const categorySales: Record<string, number> = {};
  
  mockSalesData.forEach(sale => {
    categorySales[sale.category] = (categorySales[sale.category] || 0) + sale.amount;
  });
  
  return Object.entries(categorySales)
    .map(([category, sales]) => ({ category, sales }))
    .sort((a, b) => b.sales - a.sales);
};

// Get sales by region data for the chart
export const getSalesByRegionData = (): SalesByRegion[] => {
  const regionSales: Record<string, number> = {};
  
  mockSalesData.forEach(sale => {
    regionSales[sale.region] = (regionSales[sale.region] || 0) + sale.amount;
  });
  
  return Object.entries(regionSales)
    .map(([region, sales]) => ({ region, sales }))
    .sort((a, b) => b.sales - a.sales);
};

// Get all sales data
export const getAllSalesData = (): SalesData[] => {
  return mockSalesData;
};
 [require("tailwindcss-animate")],
} satisfies Config;


import React, { useState } from 'react';
import { 
  getKPIData, 
  getMonthlySalesData, 
  getSalesByCategoryData, 
  getSalesByRegionData 
} from '@/services/salesData';
import KPICard from '@/components/dashboard/KPICard';
import MonthlyChart from '@/components/dashboard/MonthlyChart';
import CategoryChart from '@/components/dashboard/CategoryChart';
import RegionChart from '@/components/dashboard/RegionChart';
import Header from '@/components/dashboard/Header';
import InfoCard from '@/components/dashboard/InfoCard';
import { 
  DollarSign, 
  TrendingUp, 
  ShoppingBag, 
  Map 
} from 'lucide-react';

const formatCurrency = (value: number): string => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value);
};

const Dashboard = () => {
  const [timePeriod, setTimePeriod] = useState('12m');
  
  // In a real app, these would change based on the time period
  const kpiData = getKPIData();
  const monthlySalesData = getMonthlySalesData();
  const categoryData = getSalesByCategoryData();
  const regionData = getSalesByRegionData();
  
  return (
    <div className="min-h-screen bg-background">
      <div className="container mx-auto py-6 px-4 sm:px-6 lg:px-8">
        <Header timePeriod={timePeriod} onTimeChange={setTimePeriod} />
        
        {/* KPI Cards Row */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
          <KPICard
            title="Total Sales"
            value={formatCurrency(kpiData.totalSales)}
            icon={<DollarSign size={24} />}
          />
          <KPICard
            title="Average Sale"
            value={formatCurrency(kpiData.averageSale)}
            change={kpiData.salesGrowth}
            icon={<TrendingUp size={24} />}
          />
          <KPICard
            title="Top Product"
            value={kpiData.topProduct}
            icon={<ShoppingBag size={24} />}
          />
          <KPICard
            title="Top Region"
            value={kpiData.topRegion}
            icon={<Map size={24} />}
          />
        </div>
        
        {/* Sales Target Card */}
        <div className="mb-6">
          <InfoCard
            title="Sales Target"
            value={formatCurrency(kpiData.totalSales)}
            subtitle="Annual target: $1,500,000"
            target={kpiData.targetCompletion}
          />
        </div>
        
        {/* Charts Row */}
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
          <MonthlyChart data={monthlySalesData} />
          <CategoryChart data={categoryData} />
        </div>
        
        {/* Bottom Chart */}
        <div className="mb-6">
          <RegionChart data={regionData} />
        </div>
      </div>
    </div>
  );
};


